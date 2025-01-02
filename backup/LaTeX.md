# Tex Live下载与安装

[清华镜像CTAN文档](https://mirrors.tuna.tsinghua.edu.cn/help/CTAN/)

## 下载

[官网下载](https://www.tug.org/texlive/acquire-netinstall.html)

[清华镜像](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/)

官网速度特别慢，建议使用清华镜像。

# VS Code配置LaTeX

安装latex-workshop插件

修改设置auto Clean为onBuild

在recipes设置中的json文件下面这段作为第一段。

```json
{
    "name": "latexmk (lualatex)",
    "tools": [
        "lualatexmk"
    ]
},       
```

# Tex Live tlmgr管理器

## 管理器更新(使用清华镜像源)

```bash
tlmgr update --self --repository https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet
```

## 宏包更新(使用清华镜像源)

```bash
tlmgr update --all --repository https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet
```

# LaTeX学习与使用

latex文档[LaTeX教程](https://www.latexstudio.net/LearnLaTeX/)

latex模板[Overleaf](https://www.overleaf.com/)，可以学习别人的模板格式。

## LaTeX常用宏包

ctex：中文格式宏包

geometry：页面设置宏包

titlesec：标题设置宏包

amsmath：数学公式宏包

## textlive中添加自定义包或类

### 最简单方法

将文件放在tex同目录下

### 一劳永逸法

将文件放在texlive文件搜索的家目录下

在 cmd 命令行中输入下面命令查看文件搜索的家目录。

```bash
kpsewhich -var-value=TEXMFHOME
```

显示结果为

```bash
C:/Users/Administrator/texmf
```

然后在此目录下创建目录\tex\latex，将文件放在该目录下即可。

注：latex只搜索\tex\latex目录下的文件，所以需要将文件放在该目录下。

### 添加texlive的包搜索路径法

texlive 有两个配置文件，一个是全局配置文件一个是用户配置文件，可以通过输入下面命令查看配置文件的所在位置。

```bash
kpsewhich -a texmf.cnf
```

这里会显示两个文件，一个是全局配置文件(路径长的)一个是用户配置文件(路径短的)。在用户配置文件中的TEXMFLOCAL变量中添加搜索路径，然后重启 texlive 即可。

这里也需要在指定路径下创建\tex\latex目录。

最后使用texhash命令更新数据库。

### sty中引用图片

第一种直接放在同目录下

第二、三种放在\tex\latex\resources目录下