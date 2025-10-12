反编译分析软件：x64dbg
帮助理解汇编：gemini2.5pro

〖思考〗猜测：静态破解--修改了加载到内存中的二进制文件{::汇编代码}。软件编译后，二进制文件记录：函数的对应代码、数据块需求内存、相对地址偏移，CPU将其加载到内存中执行。此时内存中既有值（通过cheatengine修改）也有代码（通过x64dbg修改）。

x64dbg的界面：
A.**CPU / Disassembly Window**
	分为4列：Address    Bytes机器码    Mnemonic机器码对应汇编代码（如C3=ret）    注释--由x64dbg生成
B.**Registers Window**
	寄存器，其中RIP(instruction pointer)寄存器永远指向{下一条执行汇编代码}
	Flags寄存器记录上一次汇编代码计算结果，::ZF(zero flags)--是否=0，如是置1
C.内存原始视图（以前用cheatengine看到过）
D.堆栈

【问题1】按F9后很快“已暂停”，是由于x64dbg软件默认的调试器断点，在‘tab::断点’disable。{{部分软件通过TLS回调函数进行‘反制调试’，因此x64dbg在TLS回调函数的入口处放置了断点。}}
	&&断点通过设置INT3汇编指令实现，对应Bytes=CC，CPU发现后立刻告知调试器、并暂停执行。
【问题2】搜索文本非希望的模块，x64dbg默认搜索RIP指向的内存对应的{模块：一个.dll或.exe}。到‘tab::符号’双击rebelle 7.exe，回到CPU界面的rebelle 7.exe基地址。
	&&中断‘字符串搜索’将导致程序崩溃！
注意到‘搜索区域’比‘搜索模块’快很多，同样能找到‘we are sorry, but your registration data appear to be invalid’。
	%%为了避免联网（rebelle使用代理后，tinywall无法拦截），在clash verge设置了拦截规则。%%（已经取消，为了围绕‘we are sorry’展开分析）
‘we are sorry’对应的{反汇编}`lea r8,qword ptr ds:[346B69E]`向r8寄存器（函数的第三个param）写入qword ptr对应64位地址，写入内容为346B69E这个硬编码地址（x64dbg）发现这个地址的qword内容是‘we are sorry’

在5行前遇到了jne指令，切换ZF值，只是让‘we are sorry’不显示，弹窗和其余文字仍然显示。AI指导我要找到函数（可能是创建窗口相关）的溯源点。找到了多行
```
push rbp
push r15
push r14
push r13
push r12
push rdi
push rsi
push rbx
```
这正是栈被创建的标志。（刚刚忽略了下面一组同样的栈创建标志）这一组才是正确的。
来到‘tab::调用堆栈’第一行第三列{返回自}即栈创建的地址，设置了断点。第二行是栈的下一层，所以第n行的{返回到}和第n+1行的{返回自}相同地址。第一行返回到452dc0汇编代码是mov rcx,r13上一行即451d2a是断点地址。

### 当前位置
程序已经联网验证过‘没有许可’，我们还处在‘弹出窗口前’位置。

【问题3】怎么返回文本搜索？点击‘tab::引用’。

ws2_32.dll模块实现联网功能。验证了程序使用WSARecv，经过6次连接后，最终弹窗。

没有进展。
回看countryboy对5.1.1版本的破解。当时Rebelle分为Rebelle 5.exe和Register.exe，后者生成一个data.erf文件（在rebelle7中也有，r7将两个程序融合了）。cb修改了R5.exe的判断，使对任意完整的erf都允许；cb修改了R.exe使任意信息都生成erf。

对1000元的程序/37.5，我可以花26小时在破解。

2025-9-26 23:06:29
通过注册新账号：`bulthuisavan+romano@gmail.com 密码SanDiego`，得以深入试用界面。共三个按钮“激活产品”“试用”“购买（仅打开网页）”。向服务器发送验证请求后，“试用”的下一界面是选择版本“Rebelle 7”“Rebelle 7 Pro”，这一阶段不再进行联网验证。---->>显然：此处是破解的脆弱点。{检查：删除data.erf reg_log.dat}仍然能离线进入软件。
	其中reg_log.dat是当前登录的账号；data.erf是‘点击试用’从互联网请求的许可。

2025-9-28 21:33:04
继续漫无目的破解。
IDA free 9.2中，以“sub_地址”划分子线程，每个sub符合‘sub rsp,{数字}’结构，AI告诉我‘rsp指向当前栈位置，栈从大地址向小地址消耗，sub rsp可以为函数预留数据空间’；
IDA通过字符串找到‘选择trial版本’的绘制界面，依照AI思路查找被引用，

2025-9-29 21:37:16
由于IDA free提供的decompile功能极其好用。我分析PE文件发现.rdata中硬嵌英文文本‘email not activated’连接到sub_451D2A，由{一长串计算++逻辑、传出文本}组成，认为{逻辑部分}可以参考。
{逻辑部分}节点1（registration data == valid）产生分支1，分支2。分支1-节点2：（email_not_activated == 1）产生分支1-1，分支1-2
	分支2“We are sorry, but your registration data appear to be invalid.”
	分支1-1“This email is not activated yet”
	分支1-2 goto LABEL_212，其中212是IDA free打标签的代码段
既然sub_451D2A在逻辑中只准备了‘错误文本信息’，谁调用了它？{猜测：网络收件函数}通过cross reference找到sub_452D5A（槽函数，Qt信号将调用槽函数）
&&decompile中a1-4分别是rdi,rsi,rdx,ecx
注意到x64dbg中test ecx,ecx后je，dec（减去1） ecx后je。对应decompile代码if(a4){if(a4 == 1)}，两个跳转都是进入（失败）。然而：改掉后卡住“连接到注册服务器...”而服务器不会再发回任何信息。①
	&&函数最末尾_Unwind_Resume不被跳到，AI认为是异常时清理用。
	①因此该函数只被失败时调用，我需要重新找到成功时调用的函数。
灵感：登录成功会触发该函数吗？不会。
灵感：直接修改452D5A跳转到1DDBD30(handle_trial_license_status)导致了“Exception_Access_Violation”是由于某处内存为空白（没有服务器返回数据，比如剩余试用时间）却被读取。
意识：直接跳转到‘选择trial版本’界面。
搜索“Rebelle 7 Pro”发现```"<html><head/><body><ul style=\"margin-top: 0px; margin-bottom: 0px; margin-left: 0px; margin-right: 0px; -qt-list-indent: 1;\"><li style=\" margin-top:0px; margin-bottom:0px; margin-left:0px; margin-right:0px; -qt-block-indent:0; text-indent:0px;\">Rebelle 7 Pro with additional features:<br/>- Pigment color mixing<br/>- NanoPixel technology<br/>- Metallic Materials<br/>- Photoshop plugin<br/>- Fractal image processing<br/>- Color management</li></ul></body></html>"```显然是‘选择trial版本’界面用到的文字。
2FDF670(draw_trial_selection_dialog)<<<1DE4170调用了该绘图函数。
猜想：如果直接跳到1DE4170？同样导致Exception_Access_Violation。‘栏：调用堆栈’发现QWidgets::SetAttribute有问题，说明调用1DE4170时环境中提供合适的参数。查看call 1DE4170<<地址1D95E34从属于函数sub_1D95C30（该函数被4处引用，很有问题！）。然而：仍然存在Exception_Access_Violation.

猜想：找到Slot的位置，改变某个按钮使它连接到“aLabelPro”的slot函数。
发现‘选择trial版本’界面的创建（2FDF670）末尾使用QMetaObject::connectSlotsByName，自动连接槽。
为了学会找槽函数，参考教程https://www.cnblogs.com/czlnb/p/16937572.html跟练。
启发我用IDA::search::text::(全部匹配项)果然搜索到很多。然而Rebelle并非CrackMe，太大太多了。