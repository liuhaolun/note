```
main_sizer.Add(left_panel,1,wx.EXPAND)#把左侧挤压干净
main_sizer.Add(right_panel,10,wx.EXPAND)
```
左侧manual栏的固定大小370宽度，由最宽处（smooth touch panel）决定
```
super().__init__(parent, size=(370, 370))#由于从(10,10)开始了350长度的矩形，所以从(0,0)到(370,370)留下了10的边境
self.panel_radius = 10#这行和下面一行共同定义了圆角矩形，所以是10-10开始的圆角，圆角是向内侧切的
self.panel_rect = wx.Rect(10, 10, 350, 350)#350是长度
```
所有按钮和上一行的距离是10，和下一行的距离是0.
```
	{IN BUTTON INIT}hbox_z.Add(btn, 0, wx.TOP | wx.RIGHT, 5) #右上增加5px间距
vbox.Add(hbox_z, 0, 5)  # 并非{外间距5px}，只要5>0就能摆放到列中间。
```
1. vbox的疑惑，我注意到另一行`vbox.Add(touchPad,0,wx.ALIGN_CENTER)`，可见第三位是flag的位置，传递任意数字后（我想wx.ALIGN_CENTER|wx.ALL也就是数字加和）都触发ALIGN_CENTER, 传递特殊数字触发wx.ALL{边框padding}，下一位就是padding=10
2. `wxALIGN_CENTRE_VERTICAL will be ignored in this sizer: only horizontal alignment flags can be used in vertical sizers`可见VERTICAL在Horizontal排列中才使用。
3. 0->不参与比例放大。
**flag通过位运算传递。**{验证：128=0，不置中}，即8位（1位为2^0）。经过验证（1,3,2）wx.ALIGN_CENTER是1对应的那位。
！正是bitmask运算使得重复传递wx.LEFT（0b00010000）和wx.ALL（0b11110000）不冲突。第4321位是左右上下。