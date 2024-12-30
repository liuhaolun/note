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

#2024年11月29日
反思：好奇底层细节，浪费时间没有学习正确写法
##### 编程规范
- editor.rs > fn evaluate_event > "Handle with events"
	view.rs > fn resize > "Receive commands related to View"
- "&mut self" > "where {self.} has to be modified"
- view.rs > let _ = self.size
	if width == 0 || height == 0 {} > "Special occasions should be handled"
- for cur_row in 0..height > if let _ = get(cur_row) > "Use {pos_row} for display row count/ not get row count"

#2024年12月4日
#### Task9
问：结构不能用default? > 需要执行任务，不是单纯数据
问：local处理unwrap而非返回？>对外部调用
问：外部调用产生result？>使用let_=fn放弃处理

#2024年12月9日 
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