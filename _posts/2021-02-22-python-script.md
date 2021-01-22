---
title: 《Python快速上手 -- 繁重工作自动化》 笔记
layout: post
---



# 《Python快速上手 -- 繁重工作自动化》 笔记

目标是学会写脚本。

可能的重点： 字符串处理，文件处理，网络处理，模块引用



## 基础

*初步了解*

#### 了解

表达式是py的基本结构

支持的数学操作符： **（指数）,%（取余）,//（取整）,/,-,\*,+。优先级依次降低。

#### 数据类型

整形int()，浮点型float()，字符串str()，Boolean类型（）

数字类型之间可以用 > < =之类的比较

字符串和数字类型之间不能直接+之类的操作，需要用对应的函数转换。

字符串复制：`'Hi' * 5` -> `'HiHiHiHiHi'`

变量赋值： `a = 1`

py是动态类型语言，不需要声明变量类型

##### Boolean

只有 True(非零，非空字符串等同于True)，False（0，空字串等同于false）

可以进行与and，或or，非not操作， 

$\color{red}{注意大小写}$

#### 简单编程

'#'用来增加注释

`print()` 输出内容

`a = input()` 读入内容

`len()` 求字符串长度



## 控制流

### 代码块

不同于Java，python是缩进控制的

### 流程

条件语句

```python
if name == 'Alice' :
	print('Hi,Alice')
elif age > 1000:
	print("Oh,A Small monster")
else :
	print("End")
```

while循环

```python
while True :
	print('Input your name')
	name = input()
	if name == 'your name':
		break
```

*py既然是缩进表示代码块，如果双层循环内循环的break对应外循环的缩进会是什么结果。。`IndentationError: expected an indented block`，，，好吧*

for循环

```python
print('My name is')
for i in range(5) :
	print('LM. Time (' + str(i) + ')')
```

可以使用break（退出循环） continue（跳转到循环开始,for的话变量进入下一个），效果等同于java

range(a,b,c)可以放三个参数，a表示开始，b表示结束，c表示步进

### 引用模块

```python
import random
random.randint(1,100)
```

```python
from random import *
randint(1,100)
```

两种方式

### sys.exit() 直接结束程序



## 函数

