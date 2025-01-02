# 确定CUDA和cudnn版本

这里我是要用TensorFlow这个python库运行机器学习代码，因此要根据TensorFlow来配置CUDA和cudnn

查看[TensorFlow官网](https://tensorflow.google.cn/install/source_windows?hl=zh-cn#gpu)，选择合适的版本方案

![TensorFlow与CUDA版本对应图](https://github.com/user-attachments/assets/821b6a1c-09a0-4961-979d-f14ed2a0c162)

# 安装CUDA

打开NVDIA控制面板，点击左下角系统信息，然后点击组件

![NVDIA控制面板](https://github.com/user-attachments/assets/3a2e2d90-991b-451f-839b-c4d68a648de2)

这里的12.0是指cuda的最高版本，较低的版本也可以用，具体需要上网搜索和尝试。

![NVDIA控制面板 系统信息](https://github.com/user-attachments/assets/522f1d4f-d52e-4493-9c02-3f548951f1f2)

登录官网，下载合适的CUDA

[官网下载地址](https://developer.nvidia.com/cuda-toolkit-archive)

![CUDA下载](https://github.com/user-attachments/assets/d0c6382d-3c00-44c0-bf06-4b0a2b11afb3)

下载完后点击安装即可，选择精简安装一路next

# 安装cudnn

[官网下载cudnn](https://developer.nvidia.com/rdp/cudnn-archive)

选择与CUDA对应的cudnn版本下载

将下载好的cudnn中的文件解压到cuda的安装目录下，GPU环境就配置好了

接下来只需在conda环境中下载相应的TensorFlow版本就可以使用GPU运行机器学习程序了

# pytorch使用GPU

在[官方网站](https://pytorch.org/)如下图页面，运行显示的命令能够更好的适配你的GPU

![5](https://github.com/user-attachments/assets/6ee0f7ca-3d64-454a-a303-0c9e65ff33d5)