https://docs.astral.sh/uv/

`uv venv`后`.venv\Scripts\Activate`激活环境，在控制台显示`(FoundationStereo) PS E:\project_depth-guess\FoundationStereo`

|激活后才能使用吗？
- 下载到系统的包也需要instsll
- 有些conda默认提供的包，在uv中默认缺失
- uv pip install opencv-python 来对应 import cv2
取消激活deactivate