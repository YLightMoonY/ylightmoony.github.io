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

### 定义与调用

```python
def getAnswer(answerNumber):
	if answerNumber == 1 :
		return 'It\'s one'
	elif answerNumber == 2:
		return 'It\'s two'
	else :
		return 'Wrong number'

result = getAnswer(random.randint(1,9))
print(result)
```

当函数没有返回值则py默认返回一个None（数据类型为NoneType），可以尝试：

函数调用时可以

调用函数填参数时一般是按照顺序一一对应，不过可以自己指定将变量付给哪个参数

```python
print('Hello', end='')
```

### 作用域

写在函数内部的参数称为局部变量，对应局部作用域。

外部称为全局变量，对应全局作用域

局部变量在函数执行后销毁，也就是说在外部不能调用局部变量，而全局变量则可以在局部作用于中引用。

在函数内部不能更改全局变量值，除非在使用前先使用global声明

```python
def spam():
	global eggs
	eggs = 'spam'
```

一个参数只能有一个作用域，或局部，或全局，不能同时充当，否则会报错。

```python
eggs = 'asdf'
def spam():
	print(eggs)
	global eggs
	eggs = 'spam' # global

File "F:\work\python\Learn\chap03\sameName.py", line 4
    global eggs
    ^
SyntaxError: name 'eggs' is used prior to global declaration
---
def spam():
print(eggs) # ERROR!
	eggs = 'spam local'
eggs = 'global'

Traceback (most recent call last):
File "C:/test3784.py", line 6, in <module>
spam()
File "C:/test3784.py", line 2, in spam
print(eggs) # ERROR!
UnboundLocalError: local variable 'eggs' referenced before assignment
```

### 异常

等同于java，只是catch改为except

```python
	try:
		return 42 / divideBy
	except Exception:
        ...
```



## 列表

包含有多个数据的变量

[1,2,3,'asdf',234]

python是动态类型语言，所以一个列表可以包含有不同类型的变量，虽然。。想不到实际使用中有什么意义，大部分是同类型放到一个列表里面。

### 操作

#### 访问

同其他语言，用从0开始的下标，只是支持负数下标，-1是最后一项，-2是倒数第二项，以此类推

##### 切片

`spam[0:4]`获取下标从0到4（不包含4）的新列表。

可以省略第一个值则默认0，省略第二个值则默认到最后一个，全部省略则直接是一个新列表，等同于copy

 +（连接） *（复制） len（求长度）

del spam[2] 删除指定下标的值，后面的值依次向前

#### 使用

```python
for i in [1,2,3]: #用于for
    print(i)
a in [1,2,3] # 判断存在
a not in [1,2,3] # 判断不存在
a,b,c = [1,2,3] #多重赋值
```

#### 增删改查

```python
spam = ['hello','hi','howdy','heyas']

print(spam.index('hello'))
print(spam.index('howdy'))

# print(spam.index('asdf')) #ValueError: 'asdf' is not in list

spam.insert(2,'asdf')
spam.append('qwer')
spam.insert(12,'zxcv')
print(spam)

spam.remove('qwer')
del spam[0]
print(spam)

```

### 排序

```python
spam.sort()
print(spam)
spam.sort(reverse=True)
print(spam)
spam.append('Hasdf')
spam.sort()
print(spam)
# 默认排序按照ASCII表，所以大写直接在小写前面
# 下面的方法全部视为小写
# 不知道有没有办法自定义排序规则
spam.sort(key=str.lower)
print(spam)
```

### 字符串 元组

不可变的列表为元组，用()括起来，字符串可以视为特殊元组。

```python
(1,2,3)
(1,)
```

只包含一个元素的元组需要在后面加个`,`否则视为一个被括号括起来的值

所以很多之前介绍的字符串操作可以用在列表上

列表和元组之间可以相互转换

`list((1,2,3))` 元组-> 列表

`tuple([1,2,3])` 列表 -> 元组

### 引用

同其他语言，列表传递的是引用，基本类型传递的是值。*不过实际要看底层实现吧*

同样可以全部视为引用。

那么将不可变类型传递过去的操作类似于两只手握住同一张纸，其中一个重新赋值就是一只手拿了另一个东西，不影响第二只手还握着纸。

可变类型如列表传递过去同样是握着一张纸，更改列表中的值只是在纸上画了东西，不影响两只手还握着同一张纸，表现起来就是两个都改变了。

### 复制

```python
import copy
spam = [1,2,3]
asdf = copy.copy(spam)

qwer = [1,2,[3,4,5]]
zxcv = copy.deepcopy(qwer)
```



## 字典

### 基础

`{'a':234 , 456:'asdf'}`

用于储存键值对，类似于map。

python的字典是无序的

##### 常用方法

* keys() 获取所有键
* values() 获取所有值
* items() 获取键值对元组

##### 常用读写

```python
spam = {'a': 123, 'b' : 345}

# 直接用
print(spam['a'])
>>> KeyError: 'asdf'

print(spam['c'])

# get 没有则返回第二个参数
print(spam.get('c',0))

# 直接写
spam['c'] = asdf

# 设置默认值,如果没有对应键就添加，如果有则不作处理
spam.setdefault('d',123)
print(spam)
>>>{'a': 123, 'b': 345, 'd': 123}

spam.setdefault('d',456)
print(spam)
>>>{'a': 123, 'b': 345, 'd': 123}
```

### pprint

格式化输出工具

`pprint.pprint(...)` 格式化输出

`pprint.pformat(...)` 获取格式化字符串



## 字符串操作

*最多的任务之一*

### 处理

py的字符串可以用'' 也可以用 ""

\表示转义，同其他语言

`r'That is Carol\'s cat.'` 在字符串外加个r表示原始字符串，可以将\打印出来，只是\依旧会起作用让后面的'表示为字符，而不是结束标志。

''' """ 三个引号包含可以组成多行字符串

```python
'''Dear Alice,
Eve's cat has been arrested for catnapping, cat burglary, and extortion.
Sincerely,
Bob'''
```

也常用于多行注释

同样具有[1:5]的切片操作，不包含坐标5

同样有in，not in的判断

### 字符串方法

upper() lower() isupper() islower() 大小写转换及判断，会生成新字串

> isalpha()返回True， 如果字符串只包含字母， 并且非空；
> isalnum()返回True， 如果字符串只包含字母和数字， 并且非空；
> isdecimal()返回True， 如果字符串只包含数字字符， 并且非空；
> isspace()返回True， 如果字符串只包含空格、 制表符和换行， 并且非空；
> .istitle()返回True， 如果字符串仅包含以大写字母开头、 后面都是小写字母的单
> 词。  

startswith() endswith() 判断开始和结束

`', '.join(['cats', 'rats', 'bats'])` 将.join前面的字符串插入到后面的列表每一项之间成为一整个字符串

`'My name is Simon'.split(' ')` 将.split前面的用后面的分割

`'hello'.rjust(10) 'hello'.ljust(10) 'hello'.center(10)` 生成长度10的字符串并将'hello'左对齐，右对齐，居中

`strip() rstrip() lstrip()`删除空白，左空白，右空白

### pyperclip

`pip install pyperclip` 安装第三方模块

pyperclip.copy() pyperclip.paste() 从剪贴板复制，粘贴到剪贴板



---

$\color{red}{**自动化任务**}$

