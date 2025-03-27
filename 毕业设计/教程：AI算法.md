#2025年03月 #03月13日
有的中文教程缺少校正，内容不详，我抛弃。我找到DDPG，在openai官网。
OpenAI在2018年提供了程序‘spinning up’或许包括了训练和算法代码
https://spinningup.openai.com/en/latest/user/algorithms.html
[deep learning 无关紧要](http://ufldl.stanford.edu/tutorial/selftaughtlearning/ExerciseSelfTaughtLearning/)
#### reward&return 奖励&回报
reward是状态函数R(state_cur,action,state_after)
累计奖励：常见return有finite-horizon undiscounted/infinite-horizon discounted 区别于是否对比较早的值乘以<1系数。
	都收敛。
**Value Function 值函数** 即某一策略的回报
#### 运动空间
**discrete** 围棋和雅达利游戏，只有有限个数动作
**continuous** 现实世界
## KINDS of RL Algorithms
#### model-free
传统方法都属于此类
#### model-based
**model提供/学习** 前者::AlphaZero 后者::world models
##### Embedded
https://projector.tensorflow.org
一个数据集，定义了70000个英文单词在200个维度的距离。model的难点在于训练model：不是每个轴的意义都明确；并非明确了意义就能填入词
！我的确可以为轴孔创建模型：它拥有许多个状态，我们找到最近的下一个好状态：根据曾经的成功经验决定

#2025年03月 #03月15日
##### gymnasium
```
import gymnasium as gym
import time
env=gym.make('CartPole-v0',render_mode='human')
state,info=env.reset()
env.render()
terminated=False
while not terminated:
    action = env.action_space.sample()
    next_state,reward,terminated,truncated,info=env.step(action)
    print(next_state)
    time.sleep(0.1)
    # env.render()
env.close()
```
render函数只需要启动一次，自动更新当前状态。