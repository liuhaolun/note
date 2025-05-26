---
sticker: emoji//1f613
---
[python代码参考](https://github.com/edwinm/micropython-vfd-driver/blob/main/vfd_16.py)
	需要翻译为Arduino代码
[Arduino代码参考](https://github.com/sfxfs/ESP32-VFD-8DM-Arduino?tab=readme-ov-file)
[另一个参考提到了接线-电容](https://github.com/3KUdelta/Futaba-VFD-16bit_ESP32)
我只需要一个显示器

[POWER] 低功耗设备

第一个不能用，我改为购买自带底板的

#### 字库
 0x04,0x02,0x04,0x08,0x30, // 0ヘ
 0x00,0x00,0x36,0x36,0x00, // 5：
共有7行，5列；
交给AI，告知是二进制表示，二进制八位共有255可显示

#### 命令
模块内置的命令如下：
%% 0xed standby mode关闭
0xec running mode重新激活 %%
不应该使用standby mode
命令详见.pdf
pdf还附带内置字体
#### 启动流程
```
  digitalWrite(en, HIGH);//仅仅是供电
  delay(100);//总得留出供电时间
  digitalWrite(Reset, LOW);//根据用户手册定义，我需要首先提供LOW的reset信号
  delayMicroseconds(10);//手册要求最低10微妙
  digitalWrite(Reset, HIGH); 
  //在2ms后开始初始化命令
  digitalWrite(cs, LOW);//cs LOW开始SPI communication
  spi_write_data(0xe0);//1110 0000 DIGIT SET OF DISPLAY TIMING 命令
  delayMicroseconds(10);
  spi_write_data(0x07);//不知道做什么的=猜测：8个显示位，循环显示，计算时间用
  digitalWrite(cs, HIGH);
  delayMicroseconds(10);
  //设置亮度
  digitalWrite(cs, LOW);
  spi_write_data(0xe4);
  delayMicroseconds(10);
  spi_write_data(0x10);DIMMING QUANTITY SETTING, n = 0 to 7
  digitalWrite(cs, HIGH);
  delayMicroseconds(10);
```
LSB (Least Significant Bit)是最右侧，相对MSB是最左侧
1110 0000 -> 0LSB
```
void spi_write_data(uint8_t w_data){
	uint8_t i;
	for (i = 0; i < 8; i++){
		digitalWrite(clk, LOW);
		delayMicroseconds(1);
		if ( (w_data & 0x01) == 0x01){
			digitalWrite(din, HIGH);
		}
		else{
			digitalWrite(din, LOW);
		}
		w_data >>= 1;//首先写入LSB，依次移动
		digitalWrite(clk, HIGH);//结束clk，写入下一个
	}//对于7个纵向bit
}
```
我增加了1微秒延时，或许能增加稳定性？

测试 0xbc 对应内置字库左侧箭头，这是偏移了？
啊，我明白，抄来的代码做了位移
`spi_write_data(data + 0x30);//显示内容数据
0x30即0011 0000
是为了使5 (0000 0101) 移动到 0010 0101 对应到内置字库的5  

```
void write_font(uint8_t x, uint8_t y,uint8_t *s){
  uint8_t i=0;
  digitalWrite(cs, LOW); //开始传输
    spi_write_data(0x40+y);//地址寄存器起始位置
    for(i=0;i<5;i++)spi_write_data(s[i]);//这里原文是i<7，可明明只有s5数字，我想，完全不需要
  digitalWrite(cs, HIGH); //停止传输
  digitalWrite(cs, LOW);
  spi_write_data(0x20+x);
  spi_write_data(0x00+y);  
  digitalWrite(cs, HIGH);
  VFD_show();
}
```

#### 自定义字体
我有7bit有效数字 0111 1111
从LSB到MSB对应最上边一行到最下边一行

去除了多余的参数，调用统一1个参数
```
  write_font(0,PLUS);
  write_font(1,CROSS);
  write_char(2,ARROW_DOWN);
  write_char(3,ARROW_UP);
  write_char(4,DASH);
  write_font(5,TWO_FOURTH);
  write_font(6,THREE_FOURTH);
  write_font(7,CROSS_PLUS);
  write_char(0,BLANK);
```

#### setup page
问：应该包括什么？
四种状态：选中/没有选中；控温/没有控温
0没有选中，没有控温；1选中，没有控温；2没有选中，控温；3选中，控温

#### Serial.println和显示冲突？？？
#### onReceive中的某部分和显示冲突
（2024年12月22日）
delay(200);不影响显示。
switch屏蔽掉？
验证了：Serial.print和显示冲突。
不用于调试时，屏蔽Serial.begin，避免冲突

判断小×小十字不适合显示，改为“框选”、大叉“停止”
0：叉子；1：框；2：无；3：叉子+框
框对于7×5太多。
使用最上方一排代替框，使用11000100代替叉子+框，使用叉子

叉的判断优先级更高，因为它在STOP指令后也触发。