- [ ] 语言研究本篇：Bio-inspired soft robotics: Material selection, actuation, and design

格式参考：template/IEEEtran_HOWTO

产出：HW5.pdf

阅读：Zotero5篇

参考：5篇的延伸参考
#### terms
采摘水果fruit harvesting
机械化 mechanization
#### abstract
Hard-structure grasping, widely utilzed, suitable for structured env.
lack inherently complian, causing large strain : not various-shape fruits.
To expand the range, machine grasp, better effect required.
We review elements, simulate soft structure behaviour, use soft material, enhance performance, reduce algorithm complexity.
Safe grasping, avoid break, make more money.
Combine big robot arm, sensing tech. Increase mobility.
Enhance working safety of human workers, safely interact with humans.

#### introduction
目标：
	导入“苹果模拟”
		- [x] 典型、简易、代替传统：传统大规模应用“旋转机器”
		- [x] 苹果模拟的简易
	传统的结构说明
		- [x] 机器工作：需要中间的宽路，降低了果树密度
		- [x] 传统机器是单线程的，缺少concurrency能力，会遗漏部分苹果
		- [x] 如果采用大量机械臂，拆解成多个小机器采摘，能够提高总产出
		- [x] 然而：当前困难，机械臂控制复杂，成本高；
			参考5R机械臂
		- [x] 所以：使用柔性机构，减少机械臂关节数量，降低成本还能提高效率
	说明：提高效率因为：降低轨迹规划难度，减小总位移
		- [x] 树枝本身是柔性的，人手动采摘时，不需要特别精准的移动
		- [x] （现在使用的机械）却需要精准移动，因为旋转脱离的原理，和避免5R机器人的成本
		- [x] 5R机器人不是未来的发展方向
		- [x] 柔性爪可以仿照人的手动采摘
	说明：果子的脆弱性，硬结构机械臂需要控制精度更高
		- [x] 要么把果子放在正中心，要么机械爪（硬）的三爪需要分别控制
		- [x] 应力过高，果子会损坏，降低利润率
		- [x] 一种想法是改变品种来适应机械采摘，然而应该反过来
		- [x] gripper能解决这些问题，但是程度？本文
+参考Bio-inspired文章

#### procedurral modeling of apples
模仿苹果的建模
	- [x] 介绍3Gmap L-system
	- [x] 苹果的样子，在建模中蒂不被需要
	- [x] 添加了berlin noise用于模拟表皮褶皱
	- [x] 对标苹果和模拟渲染图
- [x] 然而，为了模型，需要简化

#### design of gripper
- [x] 为什么使用结构而非柔性材料？
	单电机
- [x] 摘果子需要力？
- [x] 现有很多硅胶的、tpu材料的爪子
	柔性材料~~老化、~~微小伤口
#### Result Analysis
deformed soft structure
- [x] 研究变形：目的=确定一种soft structure的配置能否导致预期中的形变
- [x] 柔性结构使用了相对硬的材料，如果不能如预期变形，会对局部压力过大
- [x] 通过调整bar的连接可以优化和设计结构
- [x] 夹持是因为产生了变形（没有表现），不断裂因为使用能形变的材料-但不能特别能变形tpu
- [x] 在图中，红色代表了应力更大
pressure distribution
- [x] 使用了3D，为什么，为什么不同
- [x] 这一图揭示了什么，哪里变形最明显？
- [x] 你怎么调整mesh密度
- [x] 这是合格的gripper吗
- [x] 哪些bar会不会断裂，结构安全吗
- [x] 它的变形是不规则的，因为苹果吗
- [x] 可以做出什么改进？下半部分更细。
- [x] 观察下半部分
- [x] 说明数据
#### conclusion and future work
- [ ] 我们进行了仿真，揭示了简单设计足以胜任苹果抓取任务
- [ ] 这种仿真提供了未来topological优化的基础
	- [ ] 优化目的是降低应力应变水平
	- [ ] 我们还可以探究soft structure收到疲劳的影响
	- [ ] 考虑改善运动传递
- [ ] 除了一种苹果，我们还可以考虑多种苹果形状
- [ ] 大量的compliant structure设计也不适合现实制作，虚拟仿真更好
- [ ] 此外，这种仿真还应该和机械臂结合起来，用来控制的力
- [ ] 机械臂的end-effector还应该包括剪刀，可以更进一步提高效率

[![bio-inspired robotics/程序化苹果 几何节点.md]]

