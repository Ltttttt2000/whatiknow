数据类型转换
```
int(x, [,base])
float(x)
complex(real, [,imag])
str(x), repr(x)表达式字符串, eval(str) 计算字符串中有效的表达式并返回对象
tuple(s), list(s) 将序列s转换成tuple/list
chr(x)整数转换为Unicode字符 
ord(x) 将字符转换成ASCII整数值
hex(x) 十六进制. oct(x) 八进制  bin(x)二进制

divmod将除法运算和取余运算结合在一起，结果返回一个tuple（元组）（商和余数）
a = 100
b = 14
print(divmod(a, b))= （7，2）
```
### String, List, Tuple公共方法
下标和切片
1. 都支持下标索引，从0开始
2. 切片
	[start: end : step]  表示“从开始索引位置的那个值计算，经过多少步长到结束索引位置”，有时候end会被省略。
	当step为负值时，表示从右往左，索引start=-1从最后一个元素，索引stop=1到第二个元素，但是不会取到该索引。
	切片不会导致越界，但通过下标访问会越界。 
	list = ['1', '2', '3', '4', '5']; print list[10:] 输出[]
	
访问会出现的问题
```
py3 访问不存在的索引或key：
字典：key访问报KeyError，get访问默认返回None
列表、元组、字符串：IndexError

py3 切片越界(索引为自然数情况)：
列表：
start越界：返回[] 空列表
end越界：返回原列表浅拷贝
start、end均越界：[] 空列表
元组：
start越界：返回() 空元组
end越界：返回原元组
start、end均越界：() 空元组
字符串：
start越界：返回'' 空字符
end越界：返回原字符串
start、end均越界：'' 空字符
```
运算符
+ 合并  两个列表/元组的加法表示两个列表进行拼接[1,2,3]+[4,5]=[1,2,3,4,5]
* 复制 truple = (1, 2, 3); print(truple*2) 元组的 “*” 运算也表示元组复制组合(1, 2, 3, 1, 2, 3)
in 判断元素是否存在
not in 元素是否不存在
cmp(item1, item2) 比较两个值
len(item), max(item), min(item), del(item)
元素交换位置 s[i],s[j] = s[j], s[i]

```
由数字，字符和下划线组成的短字符串以及[-5,256]内的整数存在内存驻留，将其赋值给多个不同的对象时，内存中只有一个副本，多个对象共享该副本
s1 = '123'；s2 = '123'；print(s1 is s2) True
a,b为字符串不可变类型，所以指向相同地址，所以  a is b 
is指地址相同  
==内容相同  
===内容和格式相同 
a = 123；b = 123；print(id(a) == id(b)) True

元组的[:]并不会创建副本，而是返回同一个对象的引用，所以t1和t2的id值均一样
t1 = (1,2,3)；t2 = t1[:]；print(t1 is t2) True

列表的[:]会创建副本，其id值发生改变，结果为False
lis1 = [1,2,3]；lis2 = lis1[:]
print(id(lis1)==id(lis2)) False

已知a = [1, 2, 3]和b = [1, 2, 4]，那么id(a[1])==id(b[1])）=True
为了提高内存利用效率对于一些简单的对象，如一些数值较小的int对象，字符串对象等，python采取重用对象内存的办法.

list1 = {'1':1,'2':2}
list2 = list1
list1['1'] = 5
sum = list1['1'] + list2['1']
print(sum) = 10
b = a: 赋值引用，a 和 b 都指向同一个对象。 list1 和 list2 指向的是同一块内存空间 
list1['1']=5  ------>     改变了这一块内存空间中'1'的value值 
执行这一步后内存空间存储的数据变为：{'1': 5, '2': 2} 
因此 sum = list1['1']+list2['1']=5+5=10 
```
布尔
```
a = 0 or 1 and True
print(a)=True
0的布尔值为False，所以0 or 1的结果为1,1的布尔值为True，所以1 and True的结果为True
```
### String
```python
# 查找子串
str.find("it") # 返回第一次出现的下标，找不到返回-1
str.index("it") # 返回第一次出现的下标，找不到报错
str.count("it") # 返回出现的次数，找不到返回0

# 判定：返回True/False
str.isdigit() # 是否纯数字
str.isalpha() # 是否纯字母
str.isspace() # 是否纯空格
str.startswith("ab") # 判定是否以ab开始
str.endswith("ab") # 判断是否以ab结尾

# 替换
str.replace(substr, newstr) # 返回新的字符串，把str中的substr替换成newstr
str.strip() # 返回新字符串，去掉前后空格
str.lstrip() # 去掉前空格
str.rstrip() # 去掉后空格

# 分割
str.split(",") # 按照子串分成若干部分，返回列表
str.partition(子串) # 返回元组，按照子串分成3个部分，(子串前面的字符串，子串，子串后面的字符串）

# 大小写转换
str.upper() # 转成大写
str.lower() # 小写
str.caplitalize() # 把第一个字母转化为大写字母，其余小写
str.title() # 把每个单词的第一个字母转化为大写，其余小写 
str.swapcase() # 大小写互换

strs = 'abcd12efg'
print(strs.upper().title()) 输出'Abcd12Efg'

			  
str.center(size) # 返回新字符串，让str在长度为size的中间
len(str)
strs = 'abcd'
print(strs.center(10, '*'))
# center() 返回一个原字符串居中,并使用空格填充至长度 width 的新字符串。默认填充字符为空格。题目中填充长度 width=10, 填充字符为 ‘*’，最终的结果为 '**abcd**'

str1 = "exam is a example!" 
str2 = "exam" 
print(str1.find(str2, 7))
# 从0开始算，但是从7开始查找

strs = ' I like python '
one = strs.split(' ')
one =  ['', 'I', 'like', 'python', '']

# strip方法匹配两侧所有的不符合条件的字符（括号内指定字符串中的每个字符）
strs = 'abbacabb'
print(strs.strip('ab')) = 'c'
# 'ab'表示的是一种集合，这里是指：[ab,ba,aa,bb,aaa,bbb,abb,baa]等; strs两端，只要是包含了上述集合中的任何一个，都删除。 

```
### List
```python
list.append()
list.extend() # 将另一个集合中的元素逐一添加到列表末尾
insert(index, object) # 指定位置插入元素
del list[index]
list.pop() # 默认移除最后一个元素
list.pop(index)
list.remove("abc")
in, not in 
print([2] in [1, 2, 3]) False [2] # 表示一个列表并不是一个元素
index/count
list.sort(reverse=True) # 按从大到小排序，无返回值，直接修改原列表
lis.sort(key=fn,reverse=True) # 对字符串的逆序进行排序，参数reverse=True表示按照降序进行排序
list.reverse() # 逆序
list.count(3) # 元素为3的个数

list 删除
listname.clear() # 删除所有元素
del listname[] 或 listname.pop()# 是按照索引删除
remove是移除具体值第一个匹配项目
list.extend(seq) # 是以列表的形式追加值
list.append()# 不同lists = [1, 2, 3, 4, 5, 6]；
lists.extend([7,8,9])；list=[1,2,3,4,5,6,7,8,9]
```

### Tuple
```
可以通过下标访问元组中的元素，不允许修改元组的数据，不能删除元组的元素。
index/count
（)元素不允许改变
TypeError: 'tuple' object does not support item assignment
Python 中的 tuple 结构为 “不可变序列”，用小括号表示。为了区别数学中表示优先级的小括号，当 tuple 中只含一个元素时，需要在元素后加上逗号。 （1，）
```

### Set
set完成对list中元素去重
```python
set{}集合不能索引取值，因此会报错：TypeError: 'set' object is not subscriptable
```

### Dict
dict是用来存储键值对结构的数据的，set其实也是存储的键值对，只是默认键和值是相同的。Python中的dict和set都是通过**散列表**来实现的。
dict中的数据是无序存放的。
```python
dic[key]
dic.get(key)
len(dic) # 键值对的个数
dic.keys() # 返回包含所有key的列表
dic.values()
dic.items() # 返回(key, value)列表
dic[key]=XX
del dic[key]
del dic
dic.clear() # 清空
theCopy = kvps.copy()   # copy一份字典
# 获取字典dict中key的值：dict.get(key,default=None），如果key在dict中则返回对应的值，否则返回default的值，其中default的值可自己指定 
```
操作的时间复杂度，插入、查找和删除都可以在O(1)的时间复杂度

键的限制，只有可哈希的对象才能作为字典的键和set的值。可hash的对象即python中的不可变对象和自定义的对象。可变对象(列表、字典、集合)是不能作为字典的键和st的值的。

与list相比：list的查找和删除的时间复杂度是O(n)，添加的时间复杂度是O(1)。但是dict使用hashtable内存的开销更大。为了保证较少的冲突，hashtable的装载因子，一般要小于0.75，在python中当装载因子达到2/3的时候就会自动进行扩容。

```python
# 为输出一个字典dic = {‘a’:1,'b':2}
lis1 = ['a','b'] lis2 = [1,2] dic = dict(zip(lis1,lis2))
dic = dict(a=1,b=2)
lis = ['a','b'] dic = dict.fromkeys(lis)  dic['a'] = 1 dic['b'] = 2

'''只有当元组内的所有元素都为不可变类型的时候，才能成为字典的key，因此程序运行过程中会报错：TypeError: unhashable type: 'list''''
dicts = {}
dicts[([1, 2])] = 'abc'
print(dicts)

```

## Function
函数的嵌套：递归函数
变量：局部变量、全局变量（先声明global后修改）、同名问题
返回值return返回的都是一个对象（元组），终止代码运行；return没有返回值时，函数自动返回None 不是Null
参数：带有默认值的参数要位于最后面
```
不定长参数*args  接受多个未命名参数，以元组形式返回
**kwargs 仅接受关键字参数，以字典形式返回
```
形式参数：定义时小括号里的参数；实际参数：调用时的参数
数据类型：可变（list, dic, set) 不可变(Numbers, string, tuple)
引用：Python中函数是引用传递不是值传递
匿名函数
```
lamdba map(lambda x: x + 3, b)，数组b中每个元素加3，又得到一个新的数组c，c=[5,8]；
a if condition else b

```
轻量级循环创建列表
```
a= [x for x in range(4)]
a = [x for x in range(3, 10) if x%2==0]
```

## 文件操作
```
打开文件：f = open("filename", "访问模式“) 
访问模式有r:只读默认模式；w：写入，若不存在创建新文件； a:打开文件追加，若不存在创建新的
rb：二进制打开只读; wb, ab
r+：读写，文件指针放在开头 w+读写，若存在则覆盖否则创建 a+ rb+ wb+ ab+
关闭文件：f.close()
文件指针：f.tell() 返回一个数字表示文件指针当前所在的位置

f.write("context")
f.read() 读完所有 f.readline()一行 f.readlines() 返回列表一行一行
Attention：文件读写都会改变指针位置，读写前要考虑指针位置
对于追加权限的a+, f.seek()修改位置后只能起到read(), write() 还是会继续在文件末尾追加
文件备份：最好按数据大小读取，一次读取部分如1024*1024
```
文件相关操作
```python
os.rename("oldname", "newname")
os.remove(delname)
os.mkdir(newdir)
os.getcwd() # 获取当前目录
os.chdir("../") # 获取目录列表
os.rmdir("删除的文件夹名）
```


有一段python的编码程序如下,请问经过该编码的字符串的解码顺序是（ ）url解码 utf16 gbk
urllib.quote(line.decode("gbk").encode("utf-16"))
字符串编译的过程：gbk==>unicode==>utf16==>url解码 
字符串解码顺序为：url解码==>utf16==>unicode==>gbk 解码与编码过程相反
题目中的代码是一个编码过程：
 编码:decode() 
 解码:encode() 
 url编码:urllib.quote() 