$$\lambda x.M$$
表示fn(x)->M
$$N/x$$
表示substitute x with N
>Church-Turing thesis验证了上面的两过程能实现所有计算机运算。

/两个输入？
$$\lambda x.\lambda y.x+y$$
x输出fn(y)->x+y
因此，表示为带入M到x，N到y
$$\lambda x.\lambda y.L=(\lambda y.[M/x])N$$conditionals通过boolean
$$\lambda x.\lambda y.x $$
$$\lambda x.\lambda y.y$$
表示true和false，那么$$\lambda b.\lambda x.\lambda y.b xy$$
代入b=True,x=M,y=N
 