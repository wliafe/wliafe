# Visual Studio使用scanf和printf函数出现报错的解决方法

在文件头部加入下面这段代码	

```cpp
#define _CRT_SECURE_NO_WARNINGS
```

# 创建新文件时自动生成#define _CRT_SECURE_NO_WARNINGS的方法

在newc++file文件中加入下面这段代码

```cpp
#define _CRT_SECURE_NO_WARNINGS
```

newc++file文件位置在Microsoft Visual Studio\Common7\IDE\VC\vcprojectitems\newc++file