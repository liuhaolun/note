&在hugging face添加了我的GPU型号作为设备后，访问GGUF后缀的模型，可以看到能否本地运行。GGUF是llama.cpp提供的量化（降低参数精度）方法。
	&我顺带问了问deepseek，发现真有
	- ‘决策树蒸馏-sklearn.tree’
	- ‘SHAP/LIME解释-shap’
	- （无用，卷积本来就是图片用得多）‘Grad-CAM可视化（针对CNN）：定位关键图像区域。-tf_keras’
	- ‘神经网络激活规则提取-sklearn.cluster’
	- ‘符号回归：生成方程-gplearn.genetic’
	- ‘分区域统计-仅仅是代码分段’
	它们可以相互结合。我甚至认为不要太关心神经网络，因为我的目的是‘轴孔装配’任务本身。
发现OpenDeepSearch项目是为llama提供的搜索工具，其介绍Serper是google搜索api提供商。
	账号outlook,23
	密码25,25,s
	https://serper.dev/playground
	免费体验`431a5334b174c9cc1ff903a1dc2f8205bea43415
	每次搜索0.001美元即1分钱。
然后jina.ai可以进行搜索重拍（提高相关度）或把网页转换为大模型友好的输入。
	其api似乎0.020$每1Mtoken
	免费体验`jina_9c1aece278744e5ca077a54d15681a0dNR0Exi5sN9IDw-_Su7WE3mHct5L-
它通过liteLLM，我可以配置‘OpenAI-Compatible Endpoints’即deepseek。liteLLM规范了上百种大模型的输出为openai格式。
它提到SmolAgent是hugging face出的，能够提高推理、代码能力。
它提供本地gradio webUI界面。