（2025年04月17日）
**再次犯下了‘以视觉效果为导向’而不是‘以功能为导向’的错误。**
复杂控件通过‘继承Panel-插入panel’创建
#结论 依赖AI一键式生成代码并不可靠，确保我是代码的第一控制人。
#### 将AxisControlPanel插入left_panel
```
        hbox=wx.BoxSizer(wx.HORIZONTAL)
        hbox.Add(AxisControl(tab_manual,"Roll"),1,10)
        hbox.Add(AxisControl(tab_manual,"Pitch"),1,10)
        vbox.Add(hbox)
```
#### 在AxisControlPanel中放置按钮和SectorPanel
```
        self.sector = SectorPanel(self, axis_type, self.on_angle_change)
        self.btn_minus = wx.Button(self,label="-",size=(50,50))
        self.label_center = wx.StaticText(self, label="0")
        self.btn_plus = wx.Button(self, label="+",size=(50,50))
```
和SectorPanel的通信通过self.sector.set_center_angle完成
#### 想到可以在单独文件中测试
&我不觉得NextChat这个应用很垃圾吗？真是三流程序员的设计。

这些指示是给我的，我经过分解后传递给AI，其才能理解。
	我希望创建一个控件，其包括约150度的弧形槽、槽内运动的红色点、和弧形槽延伸出的圆形按钮。
	弧形槽的背景是灰色的，它的中心部分是凹陷下去的；
	红色点可以在槽内任意运动，当点运动到了整数度数部分，会print当前度数
	左右侧的圆形按钮恰好在弧形槽对应的圆周位置，并且大小和槽宽相同；
	在圆心有数字，初始是0，点左侧按钮数字降低，点右侧按钮数字增加。
**控件结构**
```
我们使用最新版本的wxpython和python3创建一个基于wx.Panel的控件，首先从创建占位符开始。
我们的空间中包括一个辅助的圆（不显示），所有控件的中心都落在圆的圆周上。
我们会绘制两个按钮，通过wxpython绘制的圆形按钮，放在函数draw_button中，
我们会绘制一个150度的弧形槽，函数draw_slot
绘制一个圆点，在弧形槽中，函数draw_dot
圆点可以拖动在槽内移动，圆点只能被拖动，即从圆点内向两侧移动才有效
槽宽、圆点和按钮的直径相同，用同一个变量定义，所有位置都用辅助圆的半径定义。
现在，生成占位的init和各绘制函数
```
1.辅助圆，其半径为150，左右侧按钮在0,180位置，弧形槽从20~160位置，点在圆周上、被弧形槽限制
2.点径、按钮径、槽宽，同一个变量R=10

**渲染**
`dc = wx.AutoBufferedPaintDC(self)`是draw context的自动缓存，导致黑屏=>弃用
`self.SetDoubleBuffered(True)`直接启用双缓存，在init中调用