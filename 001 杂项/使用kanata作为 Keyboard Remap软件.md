kanata分为版本：gui OR tty(teletype) terminalVer.
选用的驱动分为winIOv2或interception, 前者和AutoHotKey同路使用钩子（经验证无法找到fn，且Hypershift转发的按键，已经屏蔽的按键都无法捕捉）。
cmd_allowed 版本或许‘调用cmd的支持’。
kanata_passthru_x64.dll用于和autohotkey避免冲突。
kanata.kbd是示例配置文件
配置参考(https://github.com/jtroo/kanata/blob/main/docs/config.adoc)
### 安装interception
管理员模式的powershell输入`& "C:\Users\U\Downloads\Interception\command line installer\install-interception.exe" /install` &用于允许exe处理参数，/紧随命令。
**无法找到fn键**，结项。
已经卸载interception<<--（废弃）
安装interception是必须要的，否则在‘注册表编辑器’等需要管理员权限的位置，kanata自动失效。
### 通过autohotkey确定能否适应需求->【无法使用】
#### prompt
写一个autohotkey v2版本的脚本，使用美式键盘布局：
1、将grace键（～）默认映射为‘鼠标左键单击’，按下视为按下，弹起视为弹起；
2、将左侧ctrl+grace键映射为正常的grace键，能输入（～）
%% 3、将右侧alt+grace键映射为‘鼠标右键单击’，按下视为按下，弹起视为弹起；
4、将右侧alt+k键映射为鼠标中键向下滚动，在按住期间持续滚动，滚动速度为每秒钟14次；
5、将右侧alt+o键映射为鼠标中键向上滚动，在按住期间持续滚动，滚动速度为每秒钟14次； %%
6、将左侧ctrl映射为左侧alt，将左侧alt映射为左侧ctrl；
%% 7、将右侧alt+横向键盘数字1键映射为鼠标中键单击，按下视为按下，弹起视为弹起。 %%
	|<--上文（全部）典型地受到hypershift的fn键限制荼毒
仅ctrl和alt互换，autohotkey就不能满足，两键都成为alt
### 通过kanata满足需求
synapse{推理运行在kanata前面}以至于fn+grace=>(Syn)grace=>(Kan)左键点击。
kanata可以处理滚轮，然而期间触摸板无法移动鼠标；kanata的ctrl和alt替换后，无法通过ctrl层输入grace

&&在微软QA论坛，有人提到‘玩minecraft时我的触控板不能工作’，这精准地是发生在我玩minecraft时，我需要彻底关闭‘在打字时防误触’
	{潜在印象}雷蛇blade系列使用synaptics的玻璃触控板。所有现代触控板使用windows precision驱动。
	【尝试--可用】修改注册表项目AAPThreshold以关闭Accidental Activation Prevention[微软](https://learn.microsoft.com/en-us/windows-hardware/design/component-guidelines/touchpad-tuning-guidelines) [ss64--技术论坛](https://ss64.org/viewtopic.php?f=9&t=468)
	【尝试--可用】安装interception，检验在注册表编辑器也可使用。
#### 解决触控板上沿误触
修改AAPthreshold为1（=0是完全关闭了AAP--包括curtains的配置）
【新建】对顶部3.7mm，左右侧3.7mm，=3700单位，设置supercurtains；SuperCurtainTop
【新建】SpaceBarOffset空格底到触控板距离，应为5.4mm
windows通过HID询问到触摸板尺寸
#### 自动启动
仅仅在HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run添加路径是不够的，必须附带`--cfg "PATH of config file."`
### 保留razer synapse用于配置，关闭自动启动
# ==输入方案==
1、caplock占了最好的位置，提供了最差的价值
2、目的是在次要层拓展（缺失的）按键，分层不能再次重叠
3、ctrl alt等功能扩展键，最多换位置、不能修改
4、space提供共享的左右手按键
#### caps分流
1、tap触发鼠标左键
2、jkli作为左中右上箭头<--行为互斥：使用caps作为鼠标和导航
3、你自定asdwe这五个能碰到的按键的功能
4、ctrl+tap触发鼠标右键
5、space+tap触发鼠标中键
6、双击shift代替capslock（手机的操作逻辑） <--改为使用长按200ms，然而：长按的目的包括‘数字键盘符号’。
	|<---短敲caps，按住shift
	  `shift-to-capslock(tap-hold 300 300 lsft caps)
	  6-1、将1~9~0的横向键盘，300ms长按映射为对应符号$&.
	  6-2、实际上rsft+字母右侧的按键，使不上劲。是否应该将【、；‘也进行映射？{有些过度深入了}
#### %%r_alt键指定右手功能%%
1、backscape转为del
	|<---部分软件中alt+bspc=undo比如obsidian.
#### %%r_alt和caps的对侧手对称性%%
1、jkli指定功能后，asdw的功能？
2、qe/uo都用于鼠标滚轮
	|<---r_alt目前没有配置
#### space大面积键盘修改
1、jkli指定功能后，asdw的功能？
2、qe/uo都用于鼠标滚轮
3、caps作为mmid
4、bspc作为del

##### &&obsidian的多光标用mmid滑动触发（同vscode）
	wow! 瞧，它们像山一样。
	wow! 瞧，它们像山一样。
	wow! 瞧，它们像山一样。
	wow! 瞧，它们像山一样。
	wow! 瞧，它们像山一样。
	wow! 瞧，它们像山一样。
	wow! 瞧，它们像山一样。

#### caps代替ctrl修饰快捷键
目前使用waszxcv共7处，并bspc1处。〖思考〗认为低频使用的被删除后，总量将减少。