2025-6-2 06:09:41
#### 布局
1.背景
2.我做了什么 -- 2.1空气轴承 2.2识别模型
3.及它们的理论
4.测试
5.结论
#### 背景
|非robotics的机构有什么优势？{上下传感器分离呢？-->留到‘做了什么部分’}
|为什么要设计空气轴承呢？
|怎么不进行完整的力模拟？
1->在batch assembly尤其是simple task with large clearance（亚毫米级），主流的研究关注robotic applications。robotics研究的内容是trajectory生成，以及通过vision信息或力信息进行align。然而在complex tasks中，robotics的重复定位精度（20μm）不足够实现稳定的装配，而通过多个motorized stage，我们既能够实现自动装配，又可以确保装配轨迹呈现直线，方便了力学分析。精密轴孔装配任务是对于custom shaft进行的，其是mini-batch任务，常规方法就是一个轴孔或一类轴孔设计一种安装装置，本装置适用于圆形轴的安装。
2->总是需要一个lateral compliant structure to allow 自由的平面移动of hole parts，而Air Bearing提供了non-contact。我注意到少有或没有论文提到了air bearing作为compliant structure，这或许因为其过高的成本，而commercial方案适用于高气压（0.40MPa）、高负载。我们通过自行设计制造空气轴承，实现了低气压的floating（0.17MPa）精密轴孔装配中，力信息最大在百毫牛级别，使用thrust ball bearing作为平面compliant structure时，横向摩擦力能达到100mN以上，而空气轴承将这一典型值降低到20mN，允许我们更准确地关注到轴孔2-point contact的friction力学信息。
3->完整的力学信息模拟是可行的，但是目前能和实际结果比较匹配的模拟区间，是干涉fit或者jamming condition，2-point contact的理论建模也存在，但是目前的研究没有能解决friction的准确判断。因此，主流的方案是进行attitude prediction，结合多种信息，比如力学、光学、位移，判断shaft和hole的相对tilt angle和tilt direction。我们没有光学信息，并且compliant structure使我们失去了lateral的准确位移信息。发现：无论怎样的attitude prediction后，总要step-by-step的调整，那么我们设想能否不进行准确的预测，而是把不同的tilt direction分为Roll axis and Pitch axis, 4 labels in total, 根据标签来step-by-step adjustment。通过将回归任务转变为classification任务，准确率有前景提高，并且能够灵活地嵌入程序的判断逻辑。
#### 空气轴承
|中心孔结构是从全面孔结构转换来？节流孔呢？
|设计了装置加工等深度槽
|通过模拟实现了压力的分布解算和{避免超声区域实现自稳定}
1->Commercial solution的空气轴承，具备非常低的flow rate和数微米的gas film。一整面的porous material用于做到这点，一般是石墨。另一种方法是在pad的中心加工节流孔。我们发现：减少porous material的面积，能够对应的减小load capacity。我们在中心位置钻孔了inlet flow hole，并且填充了石膏以降低flow rate。实际使用中，启动时会有振动和尖锐爆鸣声，我们认为产生了supersonic area. 所以根据研究结果，推导了shock的发生位置。为了避免shock的发生，增加了一些cross-line and circular grooves
为了增加这些grooves，我需要一个flat surface but not as hard as glass， 我在玻璃上固化epoxy来形成flat surface, 通过一些fixtures进行grooving。
3->然后，通过有限元方法，我将圆形pad划分为grids，应用了Poiseulle's flow计算了特定pressure boundary condition下的pressure distribution。I integrated结果以获得不同height即gas film thickness下的支撑力。对于experimental equipment，我们的重要假设是：the axis of each components align with each other对于安装轴的部分，和安装孔的部分都是这样。其中：空气轴承必须不发生不均匀压缩才能满足这一假设。事实上，结果更好：我们认为空气轴承发生的压缩小于10μm，可以忽略不计。
我们测试了不同compliant structure下X-axis的sensor reading. 显然air bearing的结果最好。然而并非如同预想的是non-contact and zero friction，我们认为这和viscosity关系不大，而是equipment中通过别的方式产生了力的传导。
#### 模拟
##### 几何测量
为了产生数据来训练classification neural network，我们需要virtual environment来生成数据。在搭建virtual environment之前，需要确定可以采用的假设用于简化模型。其中，如果我们能认为shaft是perfect cylinder，2-point contact的位置就总是确定的，并且wedging（由于几何关系导致的卡死）能够简单的模拟。我们用3维测量仪测得shaft的截面更接近圆而非椭圆，因此我们不需要把shaft沿yaw轴旋转以让椭圆的长轴对齐hole. 我们发现cylinderity主要来源于central line的straightness，由于显然straight不发生跳动（shaft的表面不是粗糙的），所以我们可以确定当存在tilt angle较大时，只有edge of shaft和inner壁of hole接触产生2-point contact。
##### 刚体模拟
另一个critical assumption是：在max limited contact force下，2N，shaft不产生变形。维持点接触有助于我们简化摩擦分析，我们将只需要考虑dry sliding和fixed最大静摩擦力。事实上，2N的力太小，有限元分析的结果是，仅产生非常微小的位移。
#### 控制算法
在virtual environment中一共生成了24000 pairs of data，这些数据关联了attitude, insertion depth, 和readings of virtual sensors. 我们设置了基础的CNN layers来在20000组数据的训练快速收敛，并且在4000组测试中得到94.4%的判断准确度。由于没有进行sim-to-real process，只依赖虚拟数据训练是很高效的。然而我们需要通过别的方式提高算法的robustness。在这里，我们将最后一层fully connected layer, which map the features into labels, 替换为k-NN层，这一网络确保了当现实数据和虚拟数据偏差较大时，仍能输出结果。然而k-NN降低了准确度，因此我们采用了majority voting方法，对于相邻的六组lebels，当它们的结果一致时，才trigger motorized stage adjustment。由于attitude上的可活动空间比insertion depth上的更小，且每次insertion depth increament都会生成新的label，所以即便大量的labels没有trigger任何adjustment，我们仍然能够进行enough steps of adjustment。实际使用中的结果是，在clearance为20μm时候，在前程的插入中（700微米即70步），有一组只产生了8次调整，并且每次调整都是正确的；后程的labels判断错误率提高很多，然而调整的正确率仍然大于一半。
#### 控制软件
最后，我们只做了软件来允许operator结合model的输出结果和force sensor readings进行自行判断。Operator可以给定insertion depth，在一定范围内进行自动insertion，而当insertion被abrupt，operator可以立刻接管，实现了人机交互。由于model被用于发掘数据的隐藏关联，我们可以简化diagrams，这些图表只关注直觉能够感知的信息，屏蔽了可能干扰用户判断的数据。