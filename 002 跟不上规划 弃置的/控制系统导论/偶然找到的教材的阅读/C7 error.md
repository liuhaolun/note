Only for stable system. Difference between output and input t->infinity.
$$E(s)=R(s)-C(s)$$
关于final Value theorem的推导。
> 已知Laplace的性质L{df}=sF-f(0)
> 已知laplace的计算是积分

$$\mathscr{L}\{\dot{f}\}=\int_{-0}^{\infty} \dot{f}(t)e^{-st} \, dt_{s\to{0}}= f(\infty)-f(0)=sF(s)-f(0), s\to{0}$$

error和输入函数相关。比如：ramp的输入函数，带来1/s^2的R(s)，就有可能发散了。

跳过了一些，直接到Steady-State Error for Disturbances
不应该跳过，中途有没搞明白的。

#### unity feedback system
H(s)=1

##### Static error constants
为了方便，把常用到的三种输入unit, ramp, parabolic对应的输出
$$e_{unit}(\infty)=\frac{1}{{1+\lim_{ s \to \infty }G(s)}}=\frac{1}{1+K_{p}}$$
$$e_{ramp}(\infty)=\frac{1}{{s\to 0+s\lim_{ s \to \infty }G(s)}}=\frac{1}{1+K_{v}}$$
$$e_{parabola}(\infty)=\frac{1}{{s^2\to 0+s^2\lim_{ s \to \infty }G(s)}}=\frac{1}{1+K_{a}}$$
其中p,v,a表示position,velocity,acceleration