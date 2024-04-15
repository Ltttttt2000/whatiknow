Python：解释型语言、动态类型、面向对象、函数是第一类对象、强类型（不允许同类型相加）
Python的变量、对象以及引用
```
变量：到内存空间的一个指针，拥有指向对象连接的空间
对象：一块内存，表示它们所代表的值
引用：自动形成的从变量到对象的指针
```
Python中的作用域
```
Python 中，一个变量的作用域总是由在代码中被赋值的地方所决定。 

Python变量查找顺序：LEGB

本地作用域(Local)—>当前作用域被嵌入的本地作用域(Enclosing locals)—>全局/模块作用域(Global)—>内置作用域(Built-in)。
L：Local，局部作用域，也就是我们在函数中定义的变量； 
E：Enclosing，嵌套的父级函数的局部作用域，即包含此函数的上级函数的局部作用域，但不是全局的；
G：Globa，全局变量，就是模块级别定义的变量； 
B：Built-in，系统内置模块里面的变量，比如int, bytearray等
```
模块和包
```
在 Python 中，模块是搭建程序的一种方式。每一个 Python 代码文件都是一个模块，并可以引用  
其他的模块，比如对象和属性。

一个包含许多 Python 代码的文件夹是一个包。一个包可以包含模块和子文件夹。
```
PEP8编码规范
```
1. 顶级定义之间空两行，比如函数或者类定义
2. 方法定义、类定义与第一个方法之间，都应该空一行
3. 三引号进行注释
4. 使用PyCharm, Eclipse一般使用4个空格来缩紧代码
```
Python的相关概念
```
自省：程序运行时能够获得对象的雷python型
命名空间：namespace（全局变量、局部变量）全局变量谨慎使用

作用域：就是程序中变量与对象存在关联的那段程序，Python属于静态作用域，变量的作用域是由该变量所在程序中的位置所决定的。在 Python 中作用域被划分成四个层级，分别是：local（局部作用域），enclosing（嵌套作用域），global（全局作用域）和 built - in（内建作用域）。对于一个变量，Python 也是按照之前四个层级依次在不用的作用域中查找。
```
**赋值、浅拷贝、深拷贝的区别**
```
对象的赋值：
	简单的对象引用b=a，b是a的别名是引用，指向同一片内存。修改a也影响b (b is a = True)
	
浅拷贝：
	创建新对象，内容非原对象的引用，而是原对象内第一层对象的引用。
	形式：切片操作(b=a[:])、工厂函数(b=list(a))、copy函数(b=copy.copy(a))
	a is b = False不是同一个对象；id(a)!=id(b) 不指向同一片内存；但是当id(x) for x in a和id(x) for x in b来查看a,b中元素的地址是，两者包含的地址相同。
	在这种情况下，列表 a 和 b 是不同的对象，修改列表 b 理论上不会影响到列表 a。
	但是要注意的是，浅拷贝之所以称之为浅拷贝，是它仅仅只拷贝了一层，在列表 a 中有一个嵌套的 list，如果我们修改了它，情况就不一样了。比如：a[3].append(‘java’)，查看列表 b，会发现列表 b 也发生了变化，这是因为，我们修改了嵌套的 list，修改外层元素，会修改它的引用，让它们指向别的位置，修改嵌套列表中的元素，列表的地址并未发生变化，指向的都是用一个位置。
	
深拷贝：
	deepcopy()
	深拷贝拷贝了对象的所有元素，包括多层嵌套的元素。全新的对象。时间和空间开销要高。
	同样的对列表 a，如果使用 b = copy.deepcopy(a)，再修改列表 b 将不会影响到列表 a，不再与原来的对象有任何的关联。

！
对于非容器类型（数字、字符等）没有拷贝一说，都是原对象的引用。如果tuple变量值包含原子类型的对象，即使采用了深拷贝，也只能得到浅拷贝。
对组合对象（包含了其他对象的对象，如列表，类实例）来说的区别，而对于数字、字符串以及其他“原子”类型，没有拷贝一说，产生的都是原对象的引用。
```

调用参数是值传递还是**引用传递**
```
Python 的参数传递有：位置参数、默认参数、可变参数、关键字参数。
函数的传值到底是值传递还是引用传递，要分情况：

不可变参数：值传递
像整数和字符串这样的不可变对象，是通过拷贝进行传递的，因为你无论如何都不可能在原处改变不可变对象
虽然参数传递是按引用传递的方式进行的，但对于不可变对象来说，函数内部的修改不会影响到原始变量。

可变参数：引用传递的
当一个变量作为参数传递给函数时，实际上是将这个变量的引用（内存地址）传递给了函数，而不是变量的值。这意味着在函数内部对参数的修改会影响到原始变量的值。
比如像列表，字典这样的对象是通过引用传递、和 C 语言里面的用指针传递数组很相似，可变对象能在函数内部改变。
```

反射概念`
```
反射（Reflection）是计算机科学中的一个概念，指的是程序在运行时能够访问、检测和修改其结构（如类、方法、属性）和行为（如调用方法、访问属性）的能力。反射使得程序能够在运行时动态地获取和操作对象的信息，而不需要提前知道这些信息。
通过字符串的形式，导入模块；通过字符串的形式，去模块寻找指定函数，并执行。利用字符串的形式去对象（模块）中操作（查找/获取/删除/添加）成员，一种基于字符串的事件驱动！

在很多编程语言中，反射通常包括以下几个方面：程序可以在运行时(获取类型信息，动态创建对象，动态调用方法，访问和修改属性，动态加载类和模块)
    
在 Python 中，反射是一种强大的特性，可以通过使用内置函数（如 getattr()、setattr()、hasattr()）和 inspect模块来实现。例如，可以使用 `getattr(obj, "method_name")` 来动态调用对象的方法，或者使用 `inspect` 模块来获取类的信息。
```
## 内建**数据类型**
```
int, bool, str, list, tuple, dict
不可变：数值型、string、tuple
如果改变了变量的值，相当于新建了一个对象，而对于相同的值的对象，在内存中则只有一个对象（一个地址）a=3, b=3, 则id(a) = id(b)
可变：list, dict
进行append, +=这种操作后，值改变了变量的值，而不会新建一个对象，变量引用的地址也不会发生变化，每个对象有自己的地址a = [1,2], b = [1,2] id(a)!=id(b)
```
Python列表和字典底层原理实现
```
都是通过动态分配内存来实现（列表：指针，字典：哈希表）
列表是一种有序的可变容器，可以存储任意类型的对象。
在内存中，列表实际上是一个指向连续内存块的指针，这些内存块存储了列表元素的值。当列表需要扩容时，Python 会分配一个更大的内存块，并将原列表的元素复制到新的内存块中，然后释放原内存块。这个过程称为动态数组的扩容。

字典是一种无序的键值对集合，可以存储任意类型的键和值。在内存中，字典实际上是一个哈希表，哈希表中存储了键值对的信息。当向字典中插入新的键值对时，Python 首先计算键的哈希值，然后根据哈希值将键值对存储到哈希表中的相应位置。如果两个键的哈希值相同，称为哈希冲突，Python 会使用开放寻址法或者链地址法来解决冲突。
```
dict
```
dict.items(): 将所有字典以列表形式返回，其中项在返回时没有特殊顺序需求
dict.iteritems()：返回迭代器对象
```

is 判断的是 a 对象是否就是 b 对象，是通过 id 来判断的。
\==判断的是 a 对象的值是否和 b 对象的值相等，是通过 value 来判断的。

代码中若是修改不可变数据会抛出`TypeError`异常
print调用底层中`sys.stdout.write`方法

类方法、类实例方法、静态方法有何区别
```
类方法：类对象的方法，用@classmethod进行装饰，形参为cls，表示类对象.使用装饰器@classmethod。第一个参数必须是当前类对象，该参数名一般约定为“cls”，通过它来传递类的属性和方法（不能传实例的属性和方法）。调用：实例对象和类对象都可以调用。
实例方法：第一个参数必须是实例对象，该参数名一般约定为“self”，通过它来传递实例的属性和方法（也可以传类的属性和方法）。调用：只能由实例对象调用。类实例方法：只有实例对象可以调用，形参为self，代指对象本身
静态方法：任意函数，@staticmethod，可用对象直接调用.使用装饰器@staticmethod。参数随意，没有“self”和“cls”参数，但是方法体中不能使用类或实例的任何属性和方法。调用：实例对象和类对象都可以调用。
```
isinstance()函数来判断一个对象是否是一个已知的类型，类似type()。
区别：
type()不会认为子类是一种父类类型，不考虑继承关系；
isinstance()会认为子类是一种父类类型，考虑继承关系。


单下划线和双下划线的作用
```
__foo__：一种约定，Python内部的名字，用来区别其他用户自定义的命名，以防冲突，就是例如__init__()，__del__()，__call__()些特殊方法。
_foo：一种约定，用来指定变量私有。不能用from module import * 导入，其他方面和公有变量一样访问。
__foo：这个有真正的意义：解析器用_classname__foo来代替这个名字，以区别和其他类相同的命名，它无法直接像公有成员一样随便访问，通过对象名._类名__xxx这样的方式可以访问。
```
python中主要存在四种命名方式：  
```
1、object # 公用方法
2、_object # 半保护
 被看作是“protect”，意思是只有类对象和子类对象自己能访问到这些变量， 
在模块或类外不可以使用，不能用’from module import *’导入。  
__object 是为了避免与子类的方法名称冲突， 对于该标识符描述的方法，父 
类的方法不能轻易地被子类的方法覆盖，他们的名字实际上是 _classname__methodname。  
3、_ _ object  # 全私有，全保护  
私有成员“private”，意思是只有类对象自己能访问，连子类对象也不能访 问到这个数据，不能用’from module import *’导入。  
4、_ _ object_ _     # 内建方法，用户不要这样定义  
```

缺省参数
```
缺省参数指在调用函数的时候没有传入参数的情况下，调用默认的参数，在调用函数的同时赋值时，所传入的参数会替代默认参数。

*args :
	不定长参数，他可以表示输入参数是不确定的，可以是任意多个。收集到的是元组
	def f(x, *args):...  调用f(1,2,3,4,5) 所以args=(2,3,4,5)元组
	如果是空的则args=()
**kwargs :
	关键字参数，赋值的时候是以键 = 值的方式，参数是可以任意多对在定义函数的时候不确定会有多少参数会传入时，就可以使用两个参数。字典形式
	def f(**kwargs):...  调用f(a='lee', b='sir') 则kwargs={'a':'lee', 'b':'sir'}
```
为什么函数名可以当参数调用
```
Python中一切皆对象，函数名是函数在内存中的空间，也是一个对象
```
Pass: `在写代码时只写框架思路，具体还未实现用pass进行占位，使程序不报错，不执行任何操作`
functools：`定义高阶函数或操作，基于已有的函数定义新的函数，函数作为输入，返回也是函数`

断言
```python
assert 声明其布尔值必须为True，发生异常则False
info = {}
info['name'] = 'egon'
info['age'] = 18
# 用assert取代以上代码
assert ('name' in info) and ('age' in info)
设置一个断言目的就是要求必须实现某个条件
```
with
```python
"""
with语句的作用是通过某种方式简化异常处理，它是所谓的上下文管理器的一种
with语句会在嵌套的代码执行之后，自动关闭文件。
如果在嵌套的代码中发生异常，它能够在外部exception handler catch异常前关闭文件。
如果嵌套代码有return/continue/break语句，它同样能够关闭文件。
"""
with open('output.txt', 'w') as f:
	f.write('Hi')
```
else和finally的作用
```
如果一个 Try - exception 中，没有发生异常，即 exception 没有执行，那么将会执行 else 语句的内容。反之，如果触发了 Try - exception（异常在 exception 中被定义），那么将会执行exception中的内容，而不执行 else 中的内容。

如果 try 中的异常没有在 exception 中被指出，那么系统将会抛出 Traceback(默认错误代码）,并且终止程序，接下来的所有代码都不会被执行，但如果有 Finally 关键字，则会在程序抛出 Traceback 之前（程序最后一口气的时候），执行 finally 中的语句。这个方法在某些必须要结束的操作中颇为有用，如释放文件句柄，或释放内存空间等。
```



## Python常见代码
常用的库
```
os, time, ramdom, pymysql, threading, multiprocessing, queue
第三方库: django, flask, requests, virtualenv, selenium, scrapy, md5, re..
科学计算库: numpy, pandas, matplotlib
Python中可以实现并发的库：线程、进程、协程、threading
```
os
```python
os.path 对系统路径文件的操作
sys.path 是对解释器的系统环境参数的操作（动态的改变解释器搜索路径）
os.remove(删除文件) 
os.walk(生成目录树下的所有文件)
os.chmod(改变目录权限)
...
```
列表生成式
```python
a = [x**2 for x in range(1,11) if x%2==0]
a = [[x**i for i in [2,3,4]] for x in range(1,11) if x%2==0]
# 展开二维数组
[item for sub in lst for item in sub]
```
生成随机数的方法
```python
random.randint(a,b) # 区间内整数
np.random.rand(5) # 生成5个随机小数
random.random() # 0-1随机小数
```
**lambda, map, filter, reduce**
```python
f = lambda x,y : x+y
f(1,2) = 3
numbers = [1,2]

map(add, numbers) # 根本上是for循环；将所有结果返回一个map对象（迭代器）
map(lambda x: x+1, numbers) = [2,3]

'''将两个列表的对应项加起来，把结果返回一个列表里'''
list(map(lambda x,y:x+y, list1, list2))

'''filter'''
numbers = range(-4,4)
list(filter(lambda x:x>0, numbers))
list(filter(lambda x:x!='o', 'Rocky'))

# reduce, Python3中reduce被放入functools模块里
reduce(lambda x,y:x+y, [1,2,3,4]) = 10
是从左到右逐步运算的1+2+3+4
```

字符串格式化
```python
# 使用位置参数，索引从0开始，format里填写{}对应的参数值
msg = 'my name is {}, and age is {}'
msg.format('lt', 23)
# 使用关键字参数，字典前加**
hash = {'name':'john', 'age':23}
msg = 'my name is {name}, and age is {age}'
msg.format(**hash)
# 填充与格式化 :[填充字符][对齐方式<^>][宽度]
'{0:*<10}'.format(10) # 左对齐
```
字典和Json
```
字典
	是一种数据结构,key值只要是能hash的就行
	存入的数据不会自动排序，可以使用sort函数对字典进行排序

Json是一种数据的表现形式，字符串类型
```

read, realine, redlines
```
read: 读取整个文件
readline: 读取下一行，使用生成器方法
readlines: 读取整个文件到一个迭代器以供我们遍历
```

python相关代码
```python
a = [[1,2],[3,4],[5,6]]
# 展开列表
x = [j for i in a for j in i]
# or 
b = np.array(a).flatten().tolist()


list.sort(reverse=False) # 在list基础上修改，无返回值
res = sorted(list) # 返回新的list

# 列表排序
foo
a = sorted(foo, key=lambda x:(x[1],x[0]))

# 求两个列表的交集、差集、并集
list(set(a).intersection(set(b)))
list(set(a).union(set(b)))
list(set(a).difference(set(b))) # a有b没有

# 读取excel
pd.read_excel('name.xlsx')

# 去空格
str.replace(" ","")
list = str.spilt(" ")
res = "".jion(list)
```

给定一个Python字典，如何实现key和value的转换
```python
original_dict = {'a': 1, 'b': 2, 'c': 3}
flipped_dict = {value: key for key, value in original_dict.items()}
```
给了一个长度为m的列表，返回前n小的元素，要求比较次数尽可能小
```python
import heapq
"""
首先将列表的前 n 个元素构建成一个最大堆。然后从第 n+1 个元素开始，依次和堆顶比较，如果小于堆顶元素，则替换堆顶元素，并调整堆，保持堆的性质。最后返回堆中的元素即为前 n 小的元素。这样可以保证比较次数尽可能小，时间复杂度为 O(mlogn)，其中 m 是列表的长度。
"""
def find_smallest_elements(lst, n):
    if n >= len(lst):
        return lst
    # 将列表的前 n 个元素构建成最大堆
    heap = lst[:n]
    heapq._heapify_max(heap)  # 转换为最大堆（注意：不是 heapq.heapify()，因为 heapq 默认是最小堆）
    # 从第 n+1 个元素开始，依次和堆顶比较
    for i in range(n, len(lst)):
        if lst[i] < heap[0]:  # 如果当前元素小于堆顶元素，则替换堆顶元素，并调整堆
            heapq._heapreplace_max(heap, lst[i])
    return heap

# 测试
lst = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5, 8, 9, 7, 9]
n = 5
result = find_smallest_elements(lst, n)
print(result)
```
字符串转IP地址
```python
"""
socket.inet_aton(ip)将 IP 地址字符串转换为网络字节序的 4 字节字符串表示，然后 struct.unpack("!I", ...) 将这个字符串解析为一个无符号整数，即 IP 地址的整数表示形式。
"""
import socket
import struct
def ip_to_int(ip):
    return struct.unpack("!I", socket.inet_aton(ip))[0]
# 示例用法
ip_str = "192.168.1.1"
ip_int = ip_to_int(ip_str)
print(ip_int)

```
一行代码实现的题目
```python
# 1-100的和
sum(range(1, 101))
# 实现数值交换
a,b = b,a
# 一行代码求奇偶数
[x for x in range(10) if x%2 == 1]
# 展开列表
lst = [[1, 2], [3, 4], [5, 6]]
[j for i in lst for j in i]
# 打乱列表
import random
random.shuffle(lst)
# 反转字符串
s[::-1]
# 查看目录下所有文件
os.listdir('.')
# 去除字符串间空格
s.replace(" ","")
"".join(s.split(" "))
# 实现字符串整数列表变成整数列表
list(map(lambda a: int(a), ['1','2','3'])) # [1,2,3]
# 删除列表中重复的值
list(set(lst))
# 99乘法表
print('\n'.join([' '.join(['%s*%s=%-2s' % (j, i, i*j) for j in range(1,i+1)]) for i in range(1, 10)]))
# 找出两个列表中相同的元素
set(a) & set(b)
# 不同元素
set(a) ^ set(b)
# 合并两个字典
dic1.update(dic2)
# 字典key从小到大排序
sorted(dic.items(), key=lambda x:x[0])
```


用Python发送文件
```python
#! /usr/bin/env python
#coding=utf-8
import sys
import time
import poplib # receive
import smtplib # smtplib

# 邮件发送函数
def send_mail():
     try:
        handle = smtplib.SMTP('smtp.126.com',25)
        handle.login('XXXX@126.com','**********')
        msg = 'To: XXXX@qq.com\r\nFrom:XXXX@126.com\r\nSubject:hello\r\n'
        handle.sendmail('XXXX@126.com','XXXX@qq.com',msg)
        handle.close()
        return 1
    except:
        return 0

# 邮件接收函数
def accpet_mail():
    try:
        p=poplib.POP3('pop.126.com')
        p.user('pythontab@126.com')
        p.pass_('**********')
        ret = p.stat() #返回一个元组:(邮件数,邮件尺寸)
       #p.retr('邮件号码')方法返回一个元组:(状态信息,邮件,邮件尺寸)
    except poplib.error_proto,e:
        print "Login failed:",e
        sys.exit(1)

# 运行当前文件时，执行sendmail和accpet_mail函数
if __name__ == "__main__":
    send_mail()
    accpet_mail()
```
是否为回文a = a[::-1]
Fibonacci
```python
def fibonacci(n):
    if n <= 0:
        return []
    elif n == 1:
        return [0]
    elif n == 2:
        return [0, 1]
    else:
        fib = [0, 1]
        for i in range(2, n):
            fib.append(fib[i-1] + fib[i-2])
        return fib
# 测试
print(fibonacci(10))  # 输出前10个Fibonacci数

```
检查是否为质数
```python
import math
'''
只需要遍历从 2 到该数的平方根之间的所有质数。因为如果一个数能被一个大于 1 和小于等于它平方根的整数整除，那么必定也能被一个小于等于它的质数整除。
'''
def is_prime(n):
    if n <= 1:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False
    for i in range(3, int(math.sqrt(n)) + 1, 2):
        if n % i == 0:
            return False
    return True

# 测试
print(is_prime(17))  # True
print(is_prime(20))  # False

```
如何处理上传文件
```python
# get方法，client用get发送请求，sever接收请求
import requests
with open('test.txt', 'rb') as f:
	requests.get('http://server ip address:port', data=f)

# 服务端代码
var http = require('http');
var fs = require('fs');
var server = http.createServer(function(req, res){
    var recData = "";
    req.on('data', function(data){
    recData += data;
    })
    req.on('end', function(data){
    recData += data;
    fs.writeFile('recData.txt', recData, function(err){
    console.log('file received');
        })
    })
    res.end('hello');
    })
server.listen(端口);
```

python程序中文输出问题
```python
1. encode和decode

import os.path
import xlrd,sys

Filename=’/home/tom/Desktop/1234.xls’
if not os.path.isfile(Filename):
    raise NameError,”%s is not a valid filename”%Filename

bk=xlrd.open_workbook(Filename)
shxrange=range(bk.nsheets)
print shxrange

for x in shxrange:
    p=bk.sheets()[x].name.encode(‘utf-8′)
    print p.decode(‘utf-8′)

2. 在文件开头加上
reload(sys)
sys.setdefaultencoding(‘utf8′)
```
Python如何copy一个文件
```
shutil模块有一个copyfile函数
```
删除文件`os.remove(filename) 或者os.unlink(filename0`

## Web Scraping 
如何使用我已经知道URL地址的Python在本地保存图像？
```python 
import urllib.request
urllib.request.urlretrieve("URL", "local-filename.jpg")
```
抓去数据
```python
from bs4 import BeautifulSoup
import requests
import sys

url = 'http://www.imdb.com/chart/top'
response = requests.get(url)
soup = BeautifulSoup(response.text)
tr = soup.findChildren("tr")
tr = iter(tr)
next(tr)

for movie in tr:
	title = movie.find('td', {'class':'titleColum}).find('a').contents[0]
..
```

