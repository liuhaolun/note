uv搞不明白，我还是改用conda->不行呢
#### 内参矩阵
https://www.cnblogs.com/picassooo/p/18056657
通过标定获得，鱼眼相机像素平面和物理世界平面的关系。
我猜想：魅族相机是0，GO3从gyroflow获取

#### uv
我得逐个补全缺失modules

#知识 flash_attn的whl安装（从而绕过编译错误）
[flash_attn安装方法](https://blog.51cto.com/u_15344287/13120915)
#可改善 我在无脑尝试很久后，对照AI看了很久后，才去找教程。
	查看官方项目github，注意到只有linux编译版本|>是否有其他人的win编译，找到。
	文件名内容很多，分别是flash_attn版本“+”cuda124torch2.5.1“+”..cp312|>cp312是？那就该查找
	通过nvidia-smi获得cuda版本，pip show torch查看pytorch版本

对于540×720，运算时间大约是4分钟。
图片不能有透明度通道，（或许：无损保存，不应强制为sRGB），左右不能太远...点云生成困难（对于我的镜头校准，点云用于逆向深度为点云）并且效果很差，...如果是正面照？

#### RPBD合成