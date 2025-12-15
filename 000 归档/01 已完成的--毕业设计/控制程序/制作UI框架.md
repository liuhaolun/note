####
```
Traceback (most recent call last):
  File "E:\project_peg-in-hole\main.py", line 79, in <module>
    main()
  File "E:\project_peg-in-hole\main.py", line 73, in main
    HelloFrame()
  File "E:\project_peg-in-hole\main.py", line 21, in __init__
    left_notebook.AddPage(tab_manual,"手动")
wx._core.wxAssertionError: C++ assertion "(int)m_pages.size() == (int)::SendMessageW((((HWND)GetHWND())), (0x1300 + 4), 0, 0L)" failed at ..\..\src\msw\notebook.cpp(303) in wxNotebook::GetPageCount():
```
没有完成的窗口导致此问题，将初始化修改为
`def __init__(self): super().__init__(None)`
重点在于None

####
`right_notebook=wx.Notebook(right_panel,style=wx.NB_BOTTOM)`
设置分栏，notebook在panel之下，在tab之上。
不要关心视觉效果，这些根据平台而决定，是一键配置的。