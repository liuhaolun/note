我可以通过KeyCode::up down移动了，但每次移动伴随着全屏刷新。
这是因为saturating_sub处错误以为“对原始数的编辑”。导致无法正常移动。
太好了！（11月17日，完成了chapter3）

（11月24日）
#### 目录 不对称
powershell启动.\simple_text_editor Cargo.toml
Cargo.toml应该位于powershell当前目录
	cargo run Cargo.toml对应目录是simple_text_editor\
	在target\debug\通过powershell则是
今早5[2]开始做了[7]，解决了目录问题，显示了Cargo.toml内容

echo "text here" > text.txt 输出UTF-16，非UTF-8不能读取。

（2024年11月29日）
反思：好奇底层细节，浪费时间没有学习正确写法
##### 编程规范
- editor.rs > fn evaluate_event > "Handle with events"
	view.rs > fn resize > "Receive commands related to View"
- "&mut self" > "where {self.} has to be modified"
- view.rs > let _ = self.size
	if width == 0 || height == 0 {} > "Special occasions should be handled"
- for cur_row in 0..height > if let _ = get(cur_row) > "Use {pos_row} for display row count/ not get row count"

（2024年12月4日）
#### Task9
问：结构不能用default? > 需要执行任务，不是单纯数据
问：local处理unwrap而非返回？>对外部调用
问：外部调用产生result？>使用let_=fn放弃处理

（2024年12月9日） 
alternate screen: 退出simple-text-editor后，用户的历史记录不被破坏。
`new(){}`包括：
- 返回自己
- 初始化动作
#### Closure
`let plus_one = |x:usize|x+1;`是一个函数，接受x值，输出x+1；
```
let message = String::from("hello,");
let greet = move |name| {print!("{message},{name}");}
```
move：【周围所有】所有权传递给函数。

>教程没有讲明白Box

#### Task10
既然教程说不明白要干什么，我自行设计；
1. 我使用了分行处理长文本，这是好的；
2. 使用scroll_offset=当前滚动位置，1为基本单位；
3. 每次改变位置，re-render
4. 跟踪cursor
###### 重构：
- move_position似乎放到view里更好
- view scroll_offset; 需要expose到editor确定cursor位置
###### 顺序：
1. 把move_position移动到view，改为返回Editor::Location
2. scroll_offset应该在cursor移动后if-change
	1. 处理移动多行的逻辑困难，我只在y=0/height移动
3. scroll_offset需要用在render；match keycode
4. render_line处增加scroll_offset导致长行（多行渲染）被看作一行
	1. `current_row+self.scroll_offset.saturating_sub(1)`多渲染一行
	2. 独立处理这一行，我只需要打印‘分段的最后一段’
	3. 为了和`for current_row in 0..height`配合，需要‘独立处理’结尾后`Terminal::move_cursor_to`
	问：一个长行可以渲染出很多行？
	那么：导致的结果是，渲染3行的长行，其index在下滑2次后不被包括了（只多渲染一行）
	所以：必须知道当前第一行该渲染几行
	我暂时搁置了。
	希望获取的是：非‘减拆分渲染的行数’，而是‘被`scroll_offset`折叠的行数’，最大‘满`first_row_num_lines`’也可能小一些：当`scroll_offset`<3时常见
	这是比较复杂的逻辑，我必须先搁置。

### Task 10 重做
###### 取消折行功能
`View::render_line` `pos_row`
- 取消`pos_row`传递，该函数不会渲染多行
- 增加`x_offset`用于判断渲染内容。
	我希望：虽然滚动到右边，仅长行会被折叠，短行不会
		导致的问题是：无法判断我在哪里，当只有短行时
- `pos_row`可以删除了。它是我的巧思，用于传递“当前渲染了屏幕的数行”信息
- 每次循环，末尾判断换行的逻辑，不再需要
（由于换行逻辑缺少，第一次渲染出现了问题。）
（我的破坏性修改，导致我无法定位问题了。）=>结论：绝对不能删除。我将使用git
`let _ = Terminal::clear_line();`被放在了`move_cursor`前面。

&&一方面这些是复杂的，另一方面要乐观：复杂的东西都是简单的东西的集合，并且这种集合可以被人类的神经描述。神经远比这些都复杂。


（2025年01月05日） #结论 照抄代码没有任何用处，我照着答案改，改不出自己的风格来，且使写代码变麻烦了。

#### 记录改变
- `Editor{view,position}->Editor{View{position}}`导致了`repl:move_cursor_to(p)`需要从`view`中获取：`self.view.get_position()`
- 增加`repl:should_process, from match &event`
- 增加`mod editor_command` `enum EditorCommand={Move(KeyCode) Resize(Size) Quit}`
	- 实现`impl TryFrom<crossterm::event::Event> for EditorCommand`
- `view`分离`mod location`

2025-11-28 22:06:56
1、读取文本的框的位置‘move position’中判定‘触及边界’，当x处于最右侧，而y上下移动时，{判定：`x == width.saturating_sub(1)`}总是生效，导致x.scroll_offset随着y上下移动逐次增加。
	AI认为：通过比较cursor坐标和scroll_offset坐标，当前者超过后者时，使后者跟随前者。从而避免‘使用states进行判断，然而制造delta来修正’|>‘只有使用trigger进行判断时，应该制造delta来修正’。
1-1、cursor坐标此时是ViewPort视窗内坐标，无法让scroll_offset跟随。
	在上文KeyCode触发的y坐标修改中，将min(height,y+1)解除。
1-2、cursor和scroll_offset都需要边界。
	设置rows num为纵向边界；设置当前row的row_len为横向边界。
	1-2-1、为此，需要在buffer.rs增加可读数据rows num，需要在每次上下移动时、判断新一行row_len. 在buffer.rs新增函数len()和row_len()
	1-2-2(bug)、当我多次按下右键，使在一个长行、向右移动光标到超出ViewPort范围、以至于需要增加offset.x. 然后，我按下左键向左移动，会发现ViewPort不动，需要多次点击左键才能使ViewPort向左侧移动。  
	并且当ViewPort不动时，按下下键，我们会出现在超出第二行最大长度的位置。
2、AI有时候会提出‘无须解决的问题的解决方法’，比如UTF-8的渲染、AI建议对每个char判断宽度来决定渲染。UTF-8的确有光标位置=2x文字数量的问题，AI暂时不知道解决方法。
**1时04分**    仍然未能解决1-2-2的问题。
我将View.rs的全文丢给AI，立刻告知我：get_position函数返回给上级Editor.rs‘绝对坐标值’{然而Terminal是一个物理设备，当Editor.rs提供了超过显示器范畴的坐标，就会粘在右侧或下侧边缘}->Terminal使用屏幕坐标系。
	坐标系：绝对坐标（相对于文本），屏幕坐标，偏移坐标offset=绝对坐标-屏幕坐标。
我隐隐有些高兴。