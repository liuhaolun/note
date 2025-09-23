#### partial-fraction
使用“式匹配”方法，如
$$
\frac{32}{s(s+4)(s+8)}=\frac{K_{1}}{s}+\frac{K_{2}}{s+4}+\frac{K_{3}}{s+8}
$$
“式匹配”了1/s，那么
$$
K_{2}=\frac{32}{(s)(s+8)}_{s\to{-4}}
$$
至于这部分的原理···
#### multiple root of multiplicity 2
考虑这种情况（注意和oscillating不同）1/(s+2)^2
上面的原理得到了解释：
$$
\frac{2}{s+1}=(s+2)^2\frac{K_{1}}{s+1}+K_{2}+(s+2)K_{3}
$$
当s=-2，才能消去无关K
而K2,K3共用s=-2，使用differentiate解决。
$$
\frac{-2s}{s+1}=\frac{(s+2)s}{(s+1)^2}K_{1}+K_{3}
$$
微分的细节，谁知道呢？
#### complex and imaginary
$$
\frac{K_{1}s+K_{2}}{s^2+2s+5}=\frac{K_{1}s+K_{2}}{(s+1)^2+1}
$$
$$
Ae^{-at}\cos \omega t+\dots=\frac{A(s+a)}{(s+a)^2+\omega^2}
$$
再结合转换回f(t)后的和差化积公式，最终成为带衰减（e^-t）的sinusoid。
#### Skill-Assessment E2.1
$$
\mathscr{L}\{te^{-5t}\}=\frac{1}{(s+5)^2}
$$
SIMPLE.
#### E2.2
$$
\mathscr{L}^{-1}\left\{ \frac{10}{s(s+2)(s+3)^2} \right\}
$$
分析一下，包括了一个multiplicity=2的重复。
$$
K_{1}=\frac{10}{18},K_{2}=\frac{10}{-2},K_{3}=\frac{10}{3},K_{4}
$$
K4比较难求，我试试matlab吧。~~zpk() command~~ 这是无关于polynomials的

#### Transfer Func.
常用c output, r input，对每个c r的n次微分，都有对应的num表示时域上的传递关系。

#### Circuit
Q1: 回路：电源-线圈-电阻-电容-电源
求Vc
以逆时针为正，已知回路电压和=0
$$
-V_{t}+V_{l}+V_{r}+V_{c}=0
$$
$$
V_{t}=\frac{di}{dt}L+iR+\int_{0}^tidt \frac{{1}}{C}
$$
然而，我需要知道Vc，即需要Vc和Ic关系。在时域不好完成。
$$
V_{c}(s)=\frac{I_{c}(s)}{Cs}
$$
![[Pasted image 20241115160550.png]]
首先，分析电流：流经L的电流=i1-i2；然后是电压：在节点R1,R2,L处，电压=Ls(I2-I1)

>跳过，直到mechanical system

#### Mechanical
我们观察外界力的正向，那么内界应力（惯性力、弹性力、阻尼力）都在反方向正向。
	f向右拉动mass，则惯性力向左，左、右侧的弹簧、阻尼力都向左。
使用superposition方法，mass1的受力只和自己、左右相邻mass2、mass3相关。
	> 可以认为mechanical adjacent = electrical node, mechanical parallel = electrical loop
![[Pasted image 20241115163228.png]]
For M1: M2造成的力和f(t)均属于external，→正向
$$
K_{1}x_{1}+K_{2}x_{1}+{f_{v_{3}} \frac{dx_{1}}{dt}}+M_{1} \frac{dx^2}{dt^2}=f(t)+K_{2}x_{2}+ f_{v_{3}} \frac{dx_{2}}{dt}
$$

> 本章完成在这里足够考试，2.6开始可以拓展

（11月17日补充）
绘制流程图，想到“下一步驱动什么”，比如用力驱动X0，那么（K1+Ds）X0/F，其中F是external，（K1+Ds）是internal项。