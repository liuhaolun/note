#### Poles & Zeros of a Transfer Function
严格定义是,zoros->laplace=0,poles->laplace->inf. 然而，我们定义numerator=0为zeros，denomenator=0为poles
> unit step function的U(s)=1/s，别混进TF把s=0当pole

#### 1st order system
Tr=2.2/a
Ts=4/a或3/a
Tc=1/a

#### 2nd order
绘制：poles在复平面图上，实数部分为在实轴，虚数部分在虚轴。
$$
CE:s^2+4.2s+36=0
$$
$$
s=-2.1\pm j 11.24
$$
即二阶方程通项公式$$s=-\zeta \omega_{n}\pm \omega_{n}\sqrt{ \zeta^2-1 }$$
显然，实数项因为\omega>0所以总在坐标左侧。
这样看，似乎很显然\zeta就是分配比，同时实数部分必须为负、不然unstable了。