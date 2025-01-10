- Try “Inverse Kinematics” to simulate 4-link arm reaching
- Try “Manual Simulator” to simulate static locomotion
“APP内没有manual simulator选项”
- Try to change the joint angles in the simulation code to make smaller steps 

We got 6 func.s in our "4 link simulator. matlabapp"
- Manual Stride:代码硬编码的机器人walk
- Inv Kinematics:指定点移动
- Kalman Setup:没有arduino硬件，不能使用
- Q-learning:使用算法，测试某种移动能否达成"Phi1,2,3到endPhi1,2,3"
- Simulated Annealing:模拟退火，获取最优路径
- Walk simulated Annealing:模拟退火，取得walk的最优参数
那么，我的任务就是把walk simulated annealing的参数填入manual stride
	“APP内没有manual simulator选项”->就是操作"manual simulator"了

另外，助教课提到了，不需要提交代码；那么，图片镶嵌在文档里就了。