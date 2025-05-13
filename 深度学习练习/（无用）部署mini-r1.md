#2025年03月 #03月31日
Group Relative policy optim. GPRO, Q-Lora
with countdown game in this blog.
	-> self-verification and search abilities --- on its own
**countdown game:** targetNum和数个availableNum，通过加减乘除拼凑。
**GPRO:** 传统PRO方法的改进，去除value(policy->reward) function需要，使用rule/binary-based reward
{没看懂GPRO的目的，不影响我接着做}

#### 安装好前置
`pip install "torch==2.5.1" tensorboard "setuptools<71.0.0" --index-url https://download.pytorch.org/whl/cu121
	pytorch用于张量计算 tensorboard用于可视化 setuptools用于打包项目
	！tensorboard在pytorch的pytorch.org不可找到，[torchvision和tensorboard的结合](https://pytorch.org/tutorials/recipes/recipes/tensorboard_with_pytorch.html)
		通过pytorch.org安装torchvision；直接通过pip安装tensorboard
！我忘了通过uv而非pip进行安装
`pip install flash-attn
	高效attention计算
	[[../毕业设计/（无关）Nvidia深度猜测软件|（无关）Nvidia深度猜测软件]]找回了flash-attn安装的解决方法
```
pip install  --upgrade \
  "transformers==4.48.1" \
  "datasets==3.1.0" \
  "accelerate==1.3.0" \
  "hf-transfer==0.1.9" \
  "deepspeed==0.15.4" \
  "trl==0.14.0"
```
hugging face套件，为了使用远程的model versioning service，套件和账号必须。
`from huggingface_hub import login`
`login(token="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",add_to_git_credential=True)`
&我于2024年10月17日生成过token，不能回忆，或许是注册账号自动生成；更合理的解释是尝试克隆声音，却没跟走下去。

#2025年04月 #04月02日
#### 导入Countdown-Tasks-3to4数据库
&注意到我的typo很多，比如：role->roll, split->spilt显然是写快了。

#### 使用GPRO训练
我其实有点被下一步需要分布式h100训练吓到了。这一步需要做了，我才能理解训练的步骤。
&注意到当写入函数无参数时，我会少写'()'，这可以通过AI检查。
#想法-误区 先别去想分布式训练和所需的GPU，它们超出了我的最终需要‘训练状态识别模型，并转换为逻辑判断’，极大远离我的目前实力。恰好符合：
	2025年3月31日"""注意到：我的追求和‘期待改善’不同向。"""
[[../Reddit/LocalLLaMA/25年4月1周 随便看看 发现openDeepSearch|25年4月1周 随便看看 发现openDeepSearch]]

#2025年04月 #04月02日
由于trl自己会顺带安装依赖项torch和transformers，手动安装导致冲突，出现`TypeError: expected string or bytes-like object, got 'NoneType' 
我放弃，我无法解决。

#### 示例结果
在200步训练前，模型会自言自语判断各种内容
在200步训练-450步训练，模型列出了各种方程组合，并判断‘大或小’
思维链就是把大量内容放在`<think></think>`中，而少量答案在`<answer></answer>`展示。模型学习使用格式只需要50步。