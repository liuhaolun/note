---
sticker: emoji//1f61c
---
[MODEL]  [ColorEasyDuino](https://lckfb.com/project/detail/lckfb-coloreasyduino-lucky?param=baseInfo&collection=c429497547c44f19915dd42ecefbd38a)

我希望使用Arduino Cloud非本地编辑。
为此，我需要Arduino Cloud Agent安装在电脑。问题：只能下载到.htm文件
> 	前往github下载

/能够检测到?
[资料下载](https://pan.baidu.com/s/16ZMrMEA3ESyhZqb0Cc4msw?pwd=LCKF#list/path=%2F%E7%AB%8B%E5%88%9B%C2%B7ColorEasyDuino%E5%BC%80%E5%8F%91%E6%9D%BF%E8%B5%84%E6%96%99%2F%E7%AC%AC05%E7%AB%A0.%E3%80%90ColorEasyDuino%E3%80%91%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7&parentPath=%2F)
下载了驱动文件"CH341SER.exe"，在utf-8下乱码，盲猜安装了。![[plus/Pasted image 20241119151237.png]]
在线尝试失败了。
![[plus/CH341SER.exe]]
好吧，安装一个文件也没什么坏。
因此，agent被卸载了。

#### 配置
tool > port > COM7
tool > board > UNO

A0-A5共五个模拟（仅输入0~1023）；0-12共13个数字引脚，其中6个标注“~”的支持PWM调制；
小红按钮复位键，重启程序
其余作用未知
- 串行通信（TX/RX引脚）
- SPI通信
- I2C通信
![[plus/Pasted image 20241119151902.png]]

我想：之前破坏的arduino就是把“参考电压输入引脚”用作接地导致。

#### 程序
~~我只用一个arduino控制两边的。~~
两个arduino在控制冷、热上保持相同配置（3风扇、5半导体、6热pin）
slave连接VFD显示器，master连接按钮。（我原本打算master还要连接两个热贴，现在觉得只连接一个更加简洁。）

#### 传递命令
```
#define CMD_INCREASE  0x01
#define CMD_DECREASE  0x02
#define CMD_CONFIRM_PAUSE   0x03
#define CMD_STOP  0x04
#define CMD_SWITCH 0x05
#define CMD_SEND_DATA 0x06
```
received command将决定VFD显示内容：
设置温度|当前温度
	不显示℃
	小数位显示为点的高度
在纸书笔记中定义了‘分离设置|显示温度界面，确保两设备同时显示’

>arduino不支持type，使用uint8_t

- [x] 当前的逻辑还是单设备逻辑，需要更改为多设备逻辑

>serial.print不需要屏蔽。

增加case -1（无按钮按下）发送CMD_SEND_DATA

#### 接收命令
[arduino参考Wire.read()](https://reference.arduino.cc/reference/en/language/functions/communication/wire/read/)
接收到命令后应该切换到对应界面

##### setup page计时器
我在每次把is_setup_page=1后，需要重置计时器

##### 显示内容
~~is_start=0显示STOP~~
is_start=0同样进入setup page
setup page需要显示“是否在进行”->叉子

###### 字库
↓ ↑ 1/4方块 2/4方块 3/4方块 4/4方块 ※ × -
或许“方块”变成小1/4也好：不合适

[字模工具](https://led.baud-dance.com)
工具不好用，还是得手画

排查了许久，原来是引脚定义错误

- [x] 现在有一些闪烁问题
- [x] 我希望把一些函数移动到.h
	我需要做的只是新建一个.ino文件，arduino自动合并
- [x] SPI是怎样实现的？我需要关闭显示的command
- [x] 使用内置字体；修正现有字库；
- [x] 延时使用standby模式（3分钟）
	这会和无操作计时器一并完成

字库中，35℃+满格相当于36℃，应去除。

>同一文件夹内ino文件不需要#include

#### I2C
我看到嘉立创的定义有SCL SDA非A4 A5
~~我的SCL对应普通UNO的A5，SDA对应A4~~
SCL，SDA在UNO R2就已经从A4、A5独立，独立后实质上和A4、A5线接

#### 计时器
我可以获取开机后经过时间
`millis()->long`
存入两个变量`cur_time``start_time`

我需要专门的`is_setup_page=1`为了什么？明白，只需要`millis()`刷新。

如果要取消，那么必须向MASTER发送信息？需要MASTER也取消。
所以，不要这个功能。

我通过`bool activate`决定按钮导致开屏幕，通过`is_standby`决定需要手动激活
`activate < timer_on`因为它和timer关系不大。

#### 判断函数determine_heat_or_cool返回显示参数


#### 命令传递
目前，温度传递不对。其余命令无法传递。
master出了问题。

是否和master的serial.print有关？
问题应该在master端，发射数据处。定义是绝对没有问题的，应为CMD_SEND_DATA没有问题。
那么问题就在我的发射逻辑。
或许和delay相关？无关
我发射了随机数，发现Slave的switch部分完美。
我决定暂时放弃I2C？？？
不对，I2C是可用的，但是我需要找到方法。

发送任意byte是可用的，基本的receiver可以保持同步。
那么：问题在于，我发送了不该发送的东西？

用了1小时解决一个问题：当电脑供电连接到slave板，master板的S1和S2按钮，电位发生偏移（相比较连接到master板）。

第二个问题是：发送温度太频繁。应该响应slave请求再发送。

第三个问题是：触发的随机性。通过把按钮接入5V解决了。

我需要request的包括：“cur_device”（保持同步）“cur_desire_temp”"cur_temp"
（2024年12月22日） request和我想的不同，是Master向Slave要几个字节的数据。
我只需要发送。》循环到最后两char

>我认为遇到的问题是“大系统的熵”。

#### PID对于慢系统很不合适
（2024年12月23日）
改为使用Fuzzy Control（用数组确定分段，给出一定的PWM值）
