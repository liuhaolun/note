（2025年04月06日）
```
DataLoader(dataset, batch_size=1, shuffle=False, sampler=None,
           batch_sampler=None, num_workers=0, collate_fn=None,
           pin_memory=False, drop_last=False, timeout=0,
           worker_init_fn=None, *, prefetch_factor=2,
           persistent_workers=False)
```
标准signature中第一重要的dataset, 可能传递1.map-style 2.iterable-style, 此处我们传递1., 要求：
1. （可选）subclass of ABSTRACT-CLASS::torch.utils.data.Dataset
2. 重写``__get__item() __len__()``
3. 如果需要non-integral indices, 自行设计DataLoader::custom sampler(see signature of DataLoader)
|在class Dataset中！transformer是？
	=>数据预处理，在我这里包括了{转为张量，随机旋转，随机移动、变大、侧拉，颜色改变，高斯模糊，均值化}，定义为``import torchvision.transforms as transforms transforms.Compose([transforms.ToTensor()..])``
	可见torchvision是处理图片的类

**Sampler**
shuffle=True进行随机sampler, 指定custom sampler通过传递sampler->signature
sampler是iterable的，所以如果dataset是iterable-style则不能使用sampler. 

**Batch**
选择batch_size=256, epoch1-step 1~101/235用时74s
64, 1~101/938用时18秒
注意到train_dataset共有60k数据，则batch_size和step之积为60k
似乎...慢了？0.11（这里一开始错算成快了<-256/64=4, 74/18=4.11）
|为什么是2的倍数？如果用非倍数呢？
	=>1~101/646用时28秒 比256慢0.04 比64慢0.07
	所以，虽然deepseek说2的倍数更适合GPU运算，它没有效果。
|如果不shuffle，是否导致模型更高错误率？（正在训练）
	=>准确度从98.15提升到98.26
	即Shuffle与否不重要，我优先关注预处理。98是比较低的准确度了。
|更大的batch_size会减少epoch中的参数更新次数，对于训练模型不好？（未解答）一般认为不会。

|交叉熵损失Cross-Entropy Loss为什么叫做这个名字？
	=>来源于信息论的交叉熵：两个概率的距离
	（二分法举例）标签‘手写数字1’概率‘是1/不是1’p=0.9, 
	$$CrossEntropyLoss=-[1·\log(0.9)+0·\log(0.1)]=-\log(0.9)≈0.105$$
	criterion标准、准则-nn.CrossEntropyLoss()被应用为当前criterion
	由于criterion的所有可用项目都支持Autograd所以可计算backward