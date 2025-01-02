# Windows环境

## 下载Miniconda

从[Conda官网](https://docs.conda.io/en/latest/miniconda.html#windows-installers)下载合适的Miniconda环境，然后安装环境。

## 利用pycharm启动Miniconda环境

![在pycharm中启动Miniconda环境](https://github.com/user-attachments/assets/ed3ce8c3-438e-48f5-abf6-c5440b7d0e5a)

## 安装基本工具

```bash
pip install jupyter torch torchvision
```

## Windows启动jupyter脚本

创建文本文件，将下述脚本复制进去，改为start_jupyter.bat文件,双击即可启动jupyter记事本

```bat
@echo off

:: 设置Miniconda路径
set conda_path=C:\Users\Administrator\miniconda3

:: 检查Miniconda是否已安装
if not exist "%conda_path%" (
    echo Miniconda未找到，请先安装Miniconda并设置正确的路径。
    pause
    exit /b
)

:: 激活Miniconda环境
call "%conda_path%\Scripts\activate.bat"

:: 启动Jupyter
start "" "%conda_path%\Scripts\jupyter-notebook.exe"

:: 退出Miniconda环境
call conda deactivate
```

# Linux环境

## 简介

我用的是Ubuntu环境，为方便配置Linux环境，我编写了一个shell脚本，脚本仓库为[pytorch-environment](https://gitee.com/wliafe/pytorch-environment.git)

## 安装python3

```bash
apt install python3.10
```

## 安装Miniconda

从[Conda官网](https://docs.conda.io/en/latest/miniconda.html#linux-installers)获取网址

![Miniconda-Linux版本下载官网图片](https://github.com/user-attachments/assets/f8e9b886-78cf-4420-9fd9-db7b384df344)

通过wget获取shell脚本，运行shell脚本

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-py310_23.5.2-0-Linux-x86_64.sh
```

## 安装基本工具

```bash
pip install jupyter torch torchvision
```