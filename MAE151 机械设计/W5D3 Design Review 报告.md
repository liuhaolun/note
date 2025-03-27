居然都第五周了，然而我还什么都没有做出成品，因此只能把‘设计图’拿出来。
{因为每周都过去了，倒也正常。}

我需要准备10分钟报告，内容包括：
- 说明：我们设计的是，它的大小 30秒
- Initial, 初始，我们的想法是满足小巧、电磁、易于组装 90秒
	重点说明易于组装，3D打印过多就会难以推广
	我们售卖套件，提供零件，DIY pack和pre-built两种
	- 然而：我们遇到了问题，由于一开始追求便宜
- 驱动方式的选择，从凸轮设计想出原理以推广到‘两种方向的凸轮’，进行了多种设计 {绘图} 90秒
	进行选component，找到‘二手电机’（用于振动）更加便宜，因此考虑了‘偏心轮驱动’
	分为两部分：1.电机传动 2.凸轮被驱动
	一开始我们进入两个误区：‘需要通过传动机构减速’和‘尽量降低成本’，因此对于在电机轴上安装什么，考虑了‘偏心轮’‘3齿或5齿的3D打印齿轮’‘8齿塑胶齿轮’
	我们通过数学论证了偏心轮驱动不可用，5齿小齿轮小于3D打印最小尺寸
	由于我们发现低频（256hz）PWM供电可以减速电机，因此使用‘8齿塑胶齿轮’。
- 制作方式的精度 60秒
	意识到后，对精度进行排列：发现有激光切割金属片，3D打印，成品件，组装，共四种精度。
	我们标记了必须进行定制的部件：凸轮、电机；以及暂时购买类似品，可以后续更新为定制部件的：dots-导光柱, 齿轮；根据‘尽量减少3D打印件’的原则，我们只用3D打印制作透明外壳。
- 选型，二次 60秒
	排除了不可行的选项后，我们的设计更加专一了。
	使用4层0.12mm激光切割金属片，错位粘贴，形成阶梯抬升0.48mm高度；
	使用成品齿轮，并且选择塑胶齿轮。{PEEK齿轮的价格和塑胶齿轮差不多，虽然它更加durable，但是太硬，不能手工安装到轴上。}
	定制了20只3.2mm空心杯电机，不带偏心轮。价格比二手电机贵，然而‘更长的引线’使测试更加容易
	使用导光柱，其长度略长于电机，因此电机可以倒置。使用激光切割金属片固定导光柱。

#### Intro: the Device we Designed, it's Standard

We can find the dimension standardization of Braille Books and Pamphlets, there are slight variations between countries and differences in electronic Braille displays. However, we can intuitively perceive their shape to be rectangulars,..., and their size to be relatively small.
#### Challenge & our Goal
We claim that the high-cost of existing Braille displays come from 3 parts, the assembly, the components, the drive methods.
#知识 ppt遇到‘不可用的字体’，使用的字体已经删除了，并且没有任何地方还在用这个。找到‘开始’-‘编辑’-‘替换↓替换字体’。
We want to replace the linear motor with rotatory motor, which is a far-more common power source in modern time. We want less components, and less 3d-printed components, because it is supposed that more 3d-print component would lead to 'less durable and less reliable' and 'harder to make'. And since what we will offer includes 2 options, the pre-build units and DIY pack, we want it to be easy to self-assemble at home without any professional equipment.
{We are replacing the **linear motor** with a **rotary motor**, a far more common and readily available power source in modern applications. This change reduces the number of components, particularly **3D-printed parts**, as they tend to be **less durable, less reliable, and harder to manufacture at scale**.}
#想法 由于没给好prompt，做出来的显然不是我想要的
#### Drive Method
The first drive method is easy to understand and easy to find-out, we could use two motors to drive two shafts, 3 cams installed on each shaft, so we can change one by one in 8 states.
Another method requires 3 motors, we divide the 6 dots into 3 pairs, and align each pair to an undulating plate, so we get 4 states to choose from.
We disposed the first method, not because it's impossible or harder to achieve, but because we think that 4 states is the upper limit, customers won't accept a display that require more time to refresh.

####
#想法-误区 我认为仍然要记忆住内容，或者‘花时间切换到拓展模式屏幕’。否则就会：缺失内容。

#### 课上讲了‘可能的生活垃圾分类方法’
教授：机器根据分类倒筒‘生活垃圾标签’，我误以为是用户贴上，自我critic后认识到是制造者贴上。
我的想法如下：1.厨余垃圾应该倒进食物残渣处理器，打碎后自然成为一袋子（然而大棒骨、硬果核需要分离） 2.可回收、不可回收往往在一个盒子里，需要拆分开 3.有害垃圾手动挑拣 4.其他垃圾一般是随时产生的
因此：没有自动分类需要。