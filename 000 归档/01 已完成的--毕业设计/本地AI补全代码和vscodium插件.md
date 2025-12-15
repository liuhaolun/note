**REST api** API规范的一种，确保机器、人可读，符合一定的文本规范。
##### auto-completion插件
continue: 开源插件，提供对话、补全功能。->它要求‘账户’，我不喜欢。=太大型了，而我偏好minimal。&包括ollama，大型而松散，不如紧凑的llama.cpp
ggml.ai，ggml技术是whisper.cpp llama.cpp背后的张量计算库。我相信它们干净好用，然而用户才38人(open-vsx下载)。

#### llama.cpp编译文件在win安装
在github界面找到，它支持一系列backend(gpu支持库)，
	|[Metal](https://github.com/ggml-org/llama.cpp/blob/master/docs/build.md#metal-build)|Apple Silicon|
	|[BLAS](https://github.com/ggml-org/llama.cpp/blob/master/docs/build.md#blas-build)|All|
	|[BLIS](https://github.com/ggml-org/llama.cpp/blob/master/docs/backend/BLIS.md)|All|
	|[SYCL](https://github.com/ggml-org/llama.cpp/blob/master/docs/backend/SYCL.md)|Intel and Nvidia GPU|
	|[MUSA](https://github.com/ggml-org/llama.cpp/blob/master/docs/build.md#musa)|Moore Threads MTT GPU|
	|[CUDA](https://github.com/ggml-org/llama.cpp/blob/master/docs/build.md#cuda)|Nvidia GPU|
	|[HIP](https://github.com/ggml-org/llama.cpp/blob/master/docs/build.md#hip)|AMD GPU|
	|[Vulkan](https://github.com/ggml-org/llama.cpp/blob/master/docs/build.md#vulkan)|GPU|
	|[CANN](https://github.com/ggml-org/llama.cpp/blob/master/docs/build.md#cann)|Ascend NPU|
	|[OpenCL](https://github.com/ggml-org/llama.cpp/blob/master/docs/backend/OPENCL.md)|Adreno GPU高通骁龙|
以及未标出的如kompute

通过nvcc --version版本是12.2，对于cuda llama-cpp提供四个版本(version=12.4or11.7,cudart-llama或llama..cuda)，我感觉cudart看着先进，下载了12.4版本，也许不能用呢？|cuda似乎不是向前兼容的？那么，下载11.7版本更不对。
	三个dll文件，我不知怎么办。
	cudart或许是原版llama的支持文件（因为它没有版本b4879）
我想用kompute版本，它很小。&godot也用了这个。&说起来，不是个好名字。
##### llama.cpp server
ggml-org在hugging face发布了转换为llama.cpp格式的Qwen2.5-coder-1.5B。使用。
`llama-server.exe -m [FILE_NAME].gguf --port 8012 -c 2048 --n-gpu-layers 99 -fa -ub 1024 -b 1024 -dt 0.1 --ctx-size 0 --cache-reuse 256
需要使用FIM-compatible models（ggml-org的定义），[插件提供的hgface列表](https://huggingface.co/collections/ggml-org/llamavim-6720fece33898ac10544ecf9)。目前只有qwen。
（2025年03月14日） FIM指fill in the middle能填入文本中间。
看来没有启动成功..kompute版本。改为cuda版本了，徘徊于细节会模糊目的。
占到一半GPU。