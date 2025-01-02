# 通用IO模型

## IO简介

本章所关注的是磁盘文件的 I/O 操作。然而，鉴于可以采用相同的系统调用对诸如管道、终端等所有类型的文件施以输入/输出操作，故而本章的大部分内容会与后续章节有关。

## 文件描述符

所有io操作的系统调用都以文件描述符（一个非负整数）指代打开的文件。

| 文件描述符 | 用途   |
|:-----:|:----:|
| 0     | 标准输入 |
| 1     | 标准输出 |
| 2     | 标准错误 |

在程序中，可以用0 1 2来表示，或者采用<unistd.h>中定义的POSIX标准名称。

**IO操作的4个系统调用**

- fd = open(pathname, flags, mode) 函数打开 pathname 所标识的文件，并返回文件描述符，用以在后续函数调用中指代打开的文件。如果文件不存在，open()函数可以创建之，这取决于对位掩码参数 flags 的设置。flags 参数还可指定文件的打开方式：只读、只写亦或是读写方式。mode 参数则指定了由 open()调用创建文件的访问权限，如果 open()函数并未创建文件，那么可以忽略或省略 mode 参数。

-  numread = read(fd, buffer, count) 调用从 fd 所指代的打开文件中读取至多 count 字节的数据，并存储到 buffer中。read()调用的返回值为实际读取到的字节数。如果再无字节可读（例如：读到文件结尾符 EOF 时），则返回值为 0。

- numwritten = write(fd, buffer, count) 调用从 buffer 中读取多达 count 字节的数据写入由fd 所指代的已打开文件中。write()调用的返回值为实际写入文件中的字节数，且有可能小于 count。

- status = close(fd)在所有输入/输出操作完成后，调用 close()，释放文件描述符 fd 以及与之相关的内核资源。

**linux通用IO**

open(),read(),write(),close()

### 打开一个文件：open()

```cpp
#include<fcntl.h>
int open(const char *pathname,int flags,\*mode*\);
```

读取成功返回文件描述符。

文件读取失败，返回-1

参数表：

![1](https://github.com/user-attachments/assets/3809173c-be93-4bc1-81c6-52e77e8b92bf)

![2](https://github.com/user-attachments/assets/c11e7785-1e74-4295-9676-1af525d33c25)

在open函数含有O_CREAT时，即需要创建文件时，就要添加mode参数注明文件的读取权限。

### 读取一个文件：read()

```cpp
#include<united.h>
ssize_t read(int fd,void *buffer, size_t count);
//成功返回实际读取字节个数
//失败返回0
```

### 数据写入文件：write()

```cpp
#include<unistd.h>
ssize_t write(int fd, void *buffer,size_t count);
//成功返回实际写入文件的字节数>=0
//如果返回0，若errno被设定，则出错，否则写入0
//返回-1，调用失败查看errno判断错误
```

### 关闭文件：close()

```cpp
#include<unistd.h>
int close(int fd);
```

### 改变文件偏移量：lseek()

```cpp
#include<unistd.h>
off_t lseek(int fd,off_t offset, int whence);
//返回文件的偏移量
```

### 通用IO模型以外的操作：ioctl()

```cpp
#include<sys/ioctl.h>
int ioctl(int fd,int request, ...)
```

### 原子操作和竞争条件

原子操作是指，某些系统调用中的所有步骤会作为独立操作而一次行加以执行，其间不会为其他进程或线程所中断。

在open函数中使用O_EXCL标志可以使打开文件成为原子操作。

### 文件控制操作：fcntl()

```cpp
#include<fcntl.h>
int fcntl(int fd, int cmd, ...);
//函数调用成功返回cmd的对应值。
//函数调用失败返回-1。
```

### 打开文件状态标志

```cpp
int flags,accessMode;
flags = fcntl(fd,F_GETFL);
if(flags == -1)
    errExit("fcntl");
```

### 文件描述符和打开文件之间的关系

多个文件描述符可以指向同一打开的文件。

内核维护的三个数据结构：

- 进程级的文件描述符表

- 系统级的打开文件表

- 文件系统的i-node表

文件描述符表：

- 控制文件描述符操作的一组标志。

- 对打开文件句柄的引用

打开文件表：

- 当前文件的偏移量

- 打开文件时所使用的状态标志

- 文件访问模式

- 与信号驱动I/O相关的设置

- 对该文件i-node对象的引用

i-node表：

- 文件类型

- 一个指针，指向该文件所持有的锁的列表

- 文件的各种属性，包括文件的大小以及与不同类型操作相关的时间戳。

### 复制文件描述符

**复制文件描述符：dup()**

```cpp
#include<unistd.h>
int dup(int oldfd);
//调用成功,返回新的文件描述符标志
//调用失败,返回-1
```

**复制文件描述符：dup2()**

```cpp
#include<unistd.h>
int dup2(int oldfd, int newfd);
//调用成功，返回新的文件描述符，与newfd相同
//调用失败，返回-1
```

若调用dup2()时，oldfd已经打开，需要调用close()将其关闭

**复制文件描述符：dup3()**

```cpp
#define _GNU_SOURCE
#include<unistd.h>
int dup3(int oldfd, int newfd, int flags);
//调用成功，返回文件描述符
//调用失败，返回-1
```

flags用于修改系统调用行为

### 在文件特定偏移量处的I/O：pread()和pwrite()

```cpp
#include<unistd.h>
ssize_t pread(int fd, void *buf, size_t count, off_t offset);
//文件描述符、变量、读取数、偏移量。
//调用成功返回读取的字节数
//调用失败返回-1
ssize_t pwrite(int fd, const void *buf, size_t count, off_t offset);
//调用成功返回写的字节数
//调用失败返回-1
```

### 分散输入和集中输出：readv()和writev()

```cpp
#include<sys/uio.h>
sturct iovec{
    void *iov_base;//起始地址
    size_t iov_len;//读取的字节数
}
ssize_t readv(int fd, const struct iovec *iov, int iovcnt);
//文件描述符、iov结构数组、iov个数
//调用成功返回读取的字节数
//调用失败返回-1
ssize_t writev(int fd, const struct iovec *iov, int iovcnt);
//调用成功返回实际写入数量
//调用失败返回-1
```

### 截断文件：truncate()和ftruncate()系统调用

作用：将文件大小设置为length参数指定的值

```cpp
#include<unistd.h>
int truncate(const char *pathname, off_t length);
int ftruncate(int fd, off_t length);
//调用成功返回0
//调用失败返回-1
```

### /dev/fd 目录

每个进程，内核都提供这个目录，这个文件目录里存储了该进程的文件描述符。

# 进程

## 进程简介

本章研究进程，重点关注进程的虚拟内存布局及内容

## 进程和程序

程序：

- 二进制格式标志

- 机器语言指令

- 程序入口地址

- 数据

- 符号表及重定位表

- 共享库和动态链接信息

- 其他信息

进程：

进程由用户内存空间和一系列内核数据结构组成

用户内存空间：程序代码及代码所使用的变量

内核数据结构：进程标识号、虚拟内存表、打开文件描述符表、信号传递及处理的有关信息、进程资源使用及限制、当前工作目录和大量的其他信息。

## 进程号和父进程号

每个进程都有一个进程号(PID)，进程号是一个正数，标识进程。

**返回该进程进程号：getpid()函数**

```cpp
#include<unistd.h>
pid_t getpid(void);
//调用成功返回PID
```

**返回该进程的父进程号：getppid()函数**

```cpp
#include<unistd.h>
pid_t getppid(void);
//调用成功返回PID
```