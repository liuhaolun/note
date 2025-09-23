（2025年02月18日）
#### 下载
https://developer.android.com/tools/releases/platform-tools?hl=zh-cn
点击 - 下载适用于 Windows 的 SDK Platform-Tools
SDK Platform-Tools 随Android版本更新，比如A13需要adb pair后使用adb connect，早先的adb没有pair指令
#### adb pair
【问题1】发现新版adb也没有pair指令？
我一直错误使用旧版adb（通过powershell的adb调用而非.\adb调用了系统路径中的）2015年10月版本1.0.32

允许通过防火墙后，成功配置了，设备名显示为windows账户名第一小节（Liu Haolun-David中Liu）
#### adb connect
connect和pair使用不同端口号，同ip号。
【疑惑】device列表包括adb-391QYECD222LX-kWorrJ.\_adb-tls-connect.\_tcp

[[adb commands]]
