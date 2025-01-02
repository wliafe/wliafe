# python的基本输入

**输入字符串**

```python
a=input()
```

**输入数字**

```python
a=eval(input())
```

**输入多个数字，中间用（特殊符号）隔开。**

```python
a=map(int,input().split())
```

split后面的括号中什么不填，默认为空格，split("")双引号中间的内容为间隔的符号。

# python的基本输出

**输出字符**

```python
print("hello world")
```

**输出变量，默认间隔为空格**

```python
a,b,c=map(int,input().split())
print(a,b,c)
```

![1](https://github.com/user-attachments/assets/f990c844-1f1f-49bb-8108-de8faba26fb3)

**输出变量，更改间隔符号**

```python
a,b,c=map(int,input().split())
print(a,b,c,sep="")
```

sep="" 引号中间为空则输出去掉空格，引号中间为（，）则间隔为逗号。

![2](https://github.com/user-attachments/assets/6a0e3862-2f34-4e11-a27b-6afcba352ac2)

![3](https://github.com/user-attachments/assets/30ebd5ba-007a-4158-86f4-a029652ac474)

**多次输出，末尾不换行，并确定间隔符**

```python
a,b,c=map(int,input().split())
print(a,end=",")
print(b,end=",")
print(c,end=",")
```

![4](https://github.com/user-attachments/assets/d611e847-a5f6-42e0-a818-bd60022aac2c)

# 基本类型转换

```python
# 将x转换为整数
int(x)
# 将x转换为浮点数
float(x)
# 将x转换为复数，实部为x，虚部为0
complex(x)
# 将x、y转换为复数，实部为x，虚部为y。
conplex(x,y)
# 将x转换为字符串
str(x)
# 将整数转换为一个字符，整数为字符的ASCII编码
chr(x)
# 将一个字符转换为他的ASCII编码的整数值
ord(x)
# 将一个整数转换为一个十六进制字符串
hex(x)
# 将一个整数转换为一个八进制字符串
oct(x)
# 将一个字符串当作有效表达式求值，返回计算结果
eval(str)
```

以上转换符用法(以int(x)为例)

```python
a=int(x)
```

# 列表

列表是一个元素的有序集合，一个列表中元素的数据类型可以各不相同，所有元素都放在[   ]中，相邻元素用逗号隔开。

## 列表的创建

```python
a_list=['physics','chemistry',2017,2.5]
b_list=['wade',3.0,81['bosh','haslem']]#列表嵌套列表
c_list=[]#创建空列表
```

## 列表读取

列表可以直接读取。

```python
a_list=[1]
```

## 列表切片

切片格式： 列表名[开始索引：结束索引：步长]

开始索引省略默认为0，结束索引省略默认为到末尾，步长省略默认为1 。

```python
a_list=['physics','chemistry',2017,2.5]
a_list=[1:3]
a_list=[:3]
a_list=[:3:2]
```

输出格式为

```python
['chemistry',2017]
['physics','chemistry',2017]
['physics',2017]
```

## 增加元素

### “+”

这种增加元素的方法是新开辟出一个空间存放新的列表，速度较慢。

这种方法不改变a_list的值。

```python
a_list=['physics','chemistry',2017,2.5]
a_list+[5]
```

输出为

```python
['physics','chemistry',2017,2.5，5]
```

### append( )

向列表尾部添加一个新元素，在原地址上操作。

```python
a_list=['physics','chemistry',2017,2.5]
a_list.append('Python')
```

输出

```python
['physics', 'chemistry', 2017, 2.5, 'Python']
```

### extend( )

将一个新列表添加到原列表的尾部，与“+”不同，extend( )是在原列表地址上操作。

```python
a_list=['physics','chemistry',2017,2.5]
a_list.extend(['Python'])
a_list.extend('Python')
```

输出

```python
['physics', 'chemistry', 2017, 2.5, 'Python']
['physics', 'chemistry', 2017, 2.5, 'P', 'y', 't', 'h', 'o', 'n']
```

### insert( )

将一个元素插入到列表的指定位置。

insert( )格式：列表名.insert(插入位置，插入元素)

当插入位置大于列表的范围时新插入的元素在列表末尾

```python
a_list=['physics','chemistry',2017,2.5]
a_list.insert(0,12.3)
a_list.insert(7,12.3)
```

输出

```python
[12.3,'chemistry',2017,2.5]
['physics', 'chemistry', 2017, 2.5, 12.3]
```

## 查找元素

### index( )

使用index可以获取指定元素首次出现的下标。

index( )格式：index（指定元素，start，end）

```python
a_list=['physics','chemistry',2017,2.5]
a_list.index(2017)
a_list.index(2017,2)
a_list.index(2017,5,7)
```

输出

【5】

### count( )

用count( )统计列表中指定元素出现次数。
格式：列表名.count(元素)

### in

使用in判断元素是否在列表中。在列表中返回True，不在返回False。

## 删除元素

### del

删除列表中指定位置元素，或整个列表。
格式：del 列表名[ ]

### remove

删除列表中首次出现的指定元素。

格式：列表名.remove(元素)

### pop

删除并返回指定位置的元素，缺少参数时删除最后一个元素。

## 列表常用函数

### 用关系运算符比较列表

列表1>列表2

从左向右比较，如果相同，比较下一个，当出现不同时返回一个值，结束比较。

最后比较的列表1的元素的值比列表2的值大时返回True,小时返回Flase。

### 函数

len(列表)：返回列表数据个数。

max，min：列表中的最大最小值。

sum：列表元素的和（必须是数字类型）

sorted：对列表升序排序。不改变原列表。

sorted(列表名，reverse=True)：降序排序。

列表名.sort()：对列表升序排序，改变原列表。

# 字符串

## 字符串相加

```python
what_he_does=' plays '
his_instrument='guitar'
his_name='Robert Johnson'
artist_intro=his_name+what_he_does+his_instrument

print(artist_intro)
```

【6】

## 字符串相乘

```python
num=3
string='name'*num
print(string)
```

![](7.png)

## 字符串替换

replace函数:string=string.replace(string[  :  ],"word")

format函数："hello { } world".format("the")

# 函数

## 函数创建

```python
def function(arg1,arg2):
	return "something"
```

## 函数调用

```python
def function(arg1,arg2):
    arg1=arg2
    return arg1
a=12
b=25
c=function(a,b)
print(c)
```

![](8.png)