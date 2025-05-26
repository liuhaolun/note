（2025年03月29日）
(https://www.kaggle.com/datasets/hojjatk/mnist-dataset/data)
- 共四组gz数据，分为train(60000组)和test(10000组)，分为images&labels
- 【可用】示例read代码(https://www.kaggle.com/code/hojjatk/read-mnist-dataset)
- 然而：不登录不能下载，我通过github找到下载(https://github.com/golbin/TensorFlow-MNIST/tree/master/mnist/data)
=>存放到E:/project_mnist/
【学习】(https://github.com/leonhardt-jon/MNIST_GUI?tab=readme-ov-file)GUI实时手写识别
####
1.创建ipynb文件，读入数据并训练
	1.1 `uv init` `uv venv` `.venv\Scripts\activate` 切换到python配置 选择.venv下的python解释器 `uv pip intall ipykernel`
	1.2 引用numpy struct array(from import array) os.path(from import join) 其中numpy需要`uv pip install numpy`
	1.2 创建MnistDataLoader类（作为实例的模板），其是object的继承。创建__init__需要self（self总被需要用于创建类）和四个path参数，存入self中同名变量，结束__init__
	1.3 创建read_image_label_pair
	`with open(path, 'rb') --- 使用read only, binary模式。with简化了file的操作，并没有隔离变量，size在外可用
		`magic, size = struct.unpack(">II",file.read(8)) --- 读取前8byte，通过struct解析为两个Integer，Integer的‘拆分为字节后存放顺序’遵循big-endian即从左到右（1234存放为0x12 0x34）（>II）。2049, 10000两个integer被拆分为II，入magic, size，magic为标识符，mnist定义了2049和2051
		`label_list=array("B",file.read()) --- 以"Byte"存放所有字节
	1.4 把array转换为矩阵，存入image_list
	`image_list.append([0]*row*col) --- 全0，长度row*col
	`img=np.array(image_data[i*row*col:(i+1)*row*col]) --- 读取到从i到i+1的长度内的字节数据
	`img=img.reshape(28,28)
	`image_list[i][:]=img --- 对当前对应i的image_list的全部（[:]取得全切片）赋值为img`
	根据deepseek-chat的表述，我注意到'img->1D array'时自动扁平化，reshape没有生效。
至此：花费1小时，获得37.5元。

（2025年04月02日）
####
2.数据集中图片显示
`%matplotlib inline`jupyter中的魔法命令，用于在行内显示
通过`uv venv mnist_env`在一个项目内生成另一个venv，重新安装`uv pip install ipykernel -U --force-reinstall`就能运行jupyter了。
重启ipykernel后，需要从开头运行以导入modules
所有路径问题，请先看‘相似却不同的文件名’(t10k-images-idx3-ubyte.gz/t10k-images.idx3-ubyte)
`plt.axis('off')`必须对每个subplot使用
至此：花费1小时，获得37.5元

（2025年04月03日）
#### 
3.CNN层设计
&我失去了之前编辑的文件，ipynb无法打开了。可见git的重要性。
	#结论 笔记应该分离到md文件中。
#知识 发觉使用卡顿，就该考虑复制-保存了。
`np.array(x_train).reshape(-1,28,28)`参数-1可以把任意个数的28×28长度向量，转换为矩阵。
我在`class Dataset(Dataset)`（Dataset引自torch.utils.data）中定义`__init__ __len__ __getitem__`具有特殊语法，`len(data_set)->len data_set[0]->getitem`

（2025年04月05日）
####
**criterion:** 模型的loss func.即模型预测结果与最优标签。
	DL中common tasks：Classification（目前）, Regression回归？, Generative Models
	在目前使用：Classification::Cross-Entropy Loss(Log Loss), 未使用focalLoss. 优化器会backpropagation以调整model parameters通过（SGD, Adam）
- Adam算法相比SGD允许参数独立学习率
- 回归任务用于预测连续值（显见和我们无关）
- focalLoss相比CE降低易分类样本权重，关注难分类样本
python中，有默认值的参数应当explicit覆盖`Adam(model.parameters(),lr=0.001)`learning rate
由于cpu的计算是缓慢的，虽然cuda我觉得难搞，仍然要使用。
	`torch.__version__`我目前安装了2.6.0+cpu版本torch
{jupyter虚拟环境}包括python kernel和ipykernel两部分，当前`uv venv mnist_env`，则vscode右上角选择的python kernel应该是\mnist_env中的，激活虚拟环境使用shell命令`mnist_env（而非.venv）\Scripts\activate`，并安装ipykernel（立刻可用无需重启）
{cuda版本不匹配？}我猜测目前安装的122版本和pytorch要求的124版本不同，所以‘仍然在cpu跑’。但不应该重装cuda，而是检查。
https://www.reddit.com/r/pytorch/comments/1gco8fd/pytorch_not_detecting_my_gpu/ ->pytorch内置了cuda，&从204mB的cpu版本和2.36gB的gpu版本可见。除非需要性能分析，否则不用安装CUDA toolkit.
=>cuda问题没有解决。我必须解决。
	1.卸载cuda toolkit后，不影响计算
	2. #知识 我第一次安装cuda toolkit导致重启且安装失败，第二次安装直接成功。猜测因为‘卸载旧版本后没有重启’导致。安装cuda toolkit后，没有加速计算->验证：torch已经内置了cuda runtime.
	3. nvidia-smi还是cuda12.2 -> 查询到和驱动程序版本对应，猜测：cuda toolkit安装时应该勾选551版display driver？
	我意识到nvidia的驱动发布是多层打包的，直接下载的GameReady或Studio驱动，包括了GforceExperience垃圾软件和真正的驱动，而官网上限制了‘真正的驱动的大多数版本不可选’，CudaToolkit打包了GFE可选项/Cuda/'true driver'。
	4.会是cuDNN的原因吗？nvidia要求注册账号才能下载，我找到(https://developer.download.nvidia.com/compute/cudnn/redist/cudnn/windows-x86_64/)直接下载。通过‘Archieve Deliverables<|搜索到的最新版下载页面’找到。&这个简单的链接list界面让人熟悉。
仍没有解决。
	资源管理器-性能-GPU 0-小箭头向下::选择cuda-始终为0%<--这是因为Jupyter选择‘运行到此’而没有运行‘本cell’
	- cuda选项必须关闭硬件加速才能看到[通过注册表](https://majorgeeks.com/content/page/hardware_accelerated_gpu_scheduling.html)
	可以占用8%的cuda，即没有问题。
**Epochs**训练轮次。
&啊！我竟然要去付费百度的‘飞浆AI社区’以获取在线GPU从而规避本地运行速度慢。1.这是个坏平台，它内容过于嘈杂 2.包装、简化是偷懒想法 3.我重新陷入算力陷阱，忽视了瓶颈在于建模能力
=>colab等在线计算不被应用，我接受缓慢的训练。

突击，赶着存了一份model. 然而计算速度等问题尚未解决。->计算速度正常。

（2025年04月06日）
[[torch.utils.data.DataLoader]]
#### train_and_validate函数
**切换到train**
nn.train()
**在train环节的每epoch中对于每batch**
optimizer.zero_grad()默认行为，清除pytorch的梯度积累
outputs=cnn(data)为图片数据进行cnn计算产生结果
loss=criterion(outputs,targets)计算Cross-Entropy Loss
loss.backward()反向影响
optimizer.step()优化
**从train切换到eval**
cnn.eval()从而‘dropout神经元’行为被停止。