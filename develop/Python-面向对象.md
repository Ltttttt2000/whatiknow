Python也有private, protected, public这些概念吗yes
面向对象
```
面向过程：根据业务逻辑从上到下写代码,基于功能分析的、以算法为中心的程序设计方法。
面向对象：将数据与函数绑定到一起，进行封装，能够快速的开发程序，减少了重复代码的过程。基于结构分析的、以数据为中心的程序设计思想。
```

类
```
类：具有相似内部状态和运动规律的实体集合或抽象，具有相同行为和属性事务的统称.
对象：某个具体存在的事物或实体
通过类可以创建对象，而对象可以具体展现出类的特性或功能
类的构成：类名+类属性+类方法
类：大驼峰命名法
class ClassName(object)
类与实例：首先定义类以后，就可以根据这个类创建出实例，所以：先定义类，然后创建实例；  
类与元类：先定义元类， 根据 metaclass 创建出类，所以：先定义 metaclass，然后创建类。
```

Python 支持以下类型的继承：
```

单继承：一个子类类继承自单个基类
多重继承：一个子类继承自多个基类
多级继承：一个子类继承自一个基类，而基类继承自另一个基类
分层继承：多个子类继承自同一个基类
混合继承：两种或两种以上继承类型的组合
```
Python 面向对象中的继承有什么特点
```
同时支持单继承与多继承，当只有一个父类时为单继承，当存在多个父类时为多继承；
子类会继承父类所有的属性和方法，子类也可以覆盖父类同名的变量和方法；
在继承中基类的构造（__init__()）方法不会被自动调用，它需要在其派生类的构造中专门调用；
在调用基类的方法时，需要加上基类的类名前缀，且需要带上 self 参数变量。区别于在类中调用普通函数时并不需要带上 self 参数。
```

面向对象的深度优先和广度优先
```
在子类继承多个父类时，属性查找方式分深度优先和广度优先两种。
- 当类是经典类时，多继承情况下，在要查找属性不存在时，会按照深度优先方式查找下去。
- 当类是新式类时，多继承情况下，在要查找属性不存在时，会按照广度优先方式查找下去。
```
OOP(Object Oriented Programming)
三大特性：**封装、继承、多态**
```
封装：将属性和方法放到一起做一个整体，然后通过实例化对象来处理，隐藏内部实现细节，只需要对象及其属性和方法交互
继承：子类（派生类）对父类（基类）属性和方法的复用
多态：定义时的类型和运行时的类型不一样
```
面向对象三个特性中，继承和多态在Python中如何体现
```
继承是指一个类（子类）可以从另一个类（父类）继承属性和方法的能力。使用 `class ChildClass(ParentClass):` 的语法来实现继承。子类可以继承父类的属性和方法，并可以添加自己的属性和方法。

多态是指不同类的对象可以对同一消息作出不同响应的能力。在 Python 中，多态通过继承和方法重写来实现。
```
Python中类的继承是深度优先继承还是广度优先继承
```
广度优先顺序进行的。
这意味着在多重继承的情况下，会先按照继承列表中从左到右的顺序依次查找方法或属性，直到找到为止。如果在某个类中找不到，则会向上查找其父类，直到找到为止。
```
魔方方法
```
__init__:对象初始化方法
__new__:创建对象时候执行的方法，单列模式会用到.静态方法，实例创建之前被调用，至少要有一个参数cls,代表要实例化的类，由解释器提供，必须有返回返回实例化出的实例，创建对象object._new_(cls)返回出创建好了的对象
__str__:当使用print输出对象时，只要自己定义了这个方法就会打印从这个方法中return的数据
__del__:删除对象时解释器默认调用，当有变量保存了一个对象的引用时，此对象的引用计数就会+1，当使用del()删除变量指向的对象时，则会减少对象的引用计数，当对象的引用计数为0时，则对象会真正被删除（内存被回收）
```

init 和__new__的区别
```
__new__在__init__之前被调用，__new__的返回值（实例）将传递给__init__方法的第一个参数，然后__init__给这个实例设置一些参数
当我们使用「类名()」创建对象的时候，Python 解释器会帮我们做两件事情：
1. 为对象在内存分配空间 __new__:
	- 为对象分配空间
	- 把对象的引用返回给 Python 解释器, 当解释器拿到了对象的引用之后，就会把对象的引用传递给 init 的第一个参数 self
1. 为对象进行初始化__init__
	- init 拿到对象的引用之后，就可以在方法的内部，针对对象来定义实例属性
```
重写New方法一定要返回对象的引用，否则Python解释器得不到分配了空间的对象引用，就不会调用对象的初始化方法。new是一个静态方法，在调用时需要主动传递cls参数
```python
# 重写 __new__ 方法
class Printer():
    def __new__(cls, *args, **kwargs):
        # 可以接收三个参数
        # 三个参数从左到右依次是 class，多值元组参数，多值的字典参数
        print("this is rewrite new") #证明了创建对象时，new方法会被自动调用；
        instance = super().__new__(cls) # 为对象分配内存空间（因为new本身就有为对象分配内存空间的能力，所以在这直接调用父类的方法即可）；
        return instance # 返回对象的引用。s
    def __init__(self):
        print("打印机初始化")
# 创建打印机对象
player = Printer()
print(player)
```
setattr, getattr
```python
setattr(self, name, value) 给name肤质
getattr(self, name) 访问
delattr(self, name) 删除

class Sample:
	def _getattr_(self, name):
		print('get')
	def _setattr_(self, name, value):
		print('set')
		self.__dict__[name] = value

s = Sample()
s.x # 输出get  s.x 这个实例属性本来是不存在的，但是由于类中有了getattr,当发现属性 x 不存在于对象的dict中时，就调用了getattr，也就是所谓的「拦截成员」。
s.x = 7 # 输出set
s.x # 7 已经存进去了

class NewRectangle:
   """
   the width and length of Rectangle
   """

   def __init__(self):
       self.width = 0
       self.length = 0

   def __setattr__(self, name, value):
       if name == 'size':
           self.width, self.length = value
       else:
           self.__dict__[name] = value

   def __getattr__(self, name):
       if name == 'size':
           return self.width, self.length
       else:
           return AttributeError

if __name__ == "__main__":
   r = NewRectangle()
   r.width = 3
   r.length = 4
   print(r.size)
   r.size = 30,40
   print(r.width)
   print(r.length)

```

抽象类和接口类
```
abstract class:
	规定了一系列的方法，并规定了必须由继承类实现的方法。由于有抽象方法的存在，所以抽象类不能实例化。可以将抽象类理解为毛坯房，门窗、墙面的样式由你自己来定，所以抽象类与作为基类的普通类的区别在于约束性更强。

interface:
	与抽象类很相似，表现在接口中定义的方法，必须由引用类实现，但他与抽象类的根本区别在于用途：与不同个体间沟通的规则（方法），你要进宿舍需要有钥匙，这个钥匙就是你与宿舍的接口，你的同室也有这个接口，所以他也能进入宿舍，你用手机通话，那么手机就是你与他人交流的接口。

interface是abstract class的变体，接口中所有的方法都是抽象的，而抽象类中可以有非抽象方法。抽象类是声明方法的存在而不去实现它的类。
接口可以继承，抽象类不可以。接口的使用方式通过 implements 关键字进行，抽象类则是通过继承 extends 关键字进行。
接口定义方法，没有实现的代码，而抽象类可以实现部分方法。
接口中基本数据类型为 static 而抽类象不是。
可以在一个类中同时实现多个接口。
```

方法重载和重写
```
overload: 方法名相同，参数不同，返回类型可以相同或不同。
override: 子类不想原封不动地继承父类的方法，而是想作一定的修改，方法覆盖
```
super
```
super() 调用父类，解决多重继承问题。MRO：类的方法解析顺序表
```

## 设计模式
**单例模式**
`单例设计模式确保一个类只有一个实例，并提供一个全局访问点。`
单例模式应用场景
```
（1）资源共享的情况下，避免由于资源操作时导致的性能或损耗等。如日志文件，应用配置。
（2）控制资源的情况下，方便资源之间的互相通信。如线程池等。 1.网站的计数器 2.应用配置 3.多线程池 4.数据库配置，数据库连接池 5.应用程序的日志应用…

比如我们每天必用的听歌软件，同一时间只能播放一首歌曲，所以对于一个听歌的软件来说，负责音乐播放的对象只有一个；再比如打印机也是同样的道理，同一时间打印机也只能打印一份文件，同理负责打印的对象也只有一个。
```
设计单例模式
```python
# 确保一个类只有一个实例存在
class Singleton:
    _instance = None  # 类变量用于存储实例
    def __new__(cls): # 静态方法，它在实例化对象时被调用
        if not cls._instance:
            cls._instance = super(Singleton, cls).__new__(cls)
        # 如果已经有了，则直接返回这个实例
        return cls._instance

# 示例用法
singleton1 = Singleton()
singleton2 = Singleton()
print(singleton1 is singleton2)  # 输出 True
```
用装饰器实现一个单例
```python
# singleton装饰器接受一个类作为参数，并返回一个闭包函数 get_instance。
def singleton(cls):
    instances = {} # 使用字典 `instances` 来保存每个类的实例
    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return get_instance

@singleton
class MyClass:
    def __init__(self, name):
        self.name = name
"""
当调用被装饰的类时，首先检查该类是否在 `instances` 中，如果不在，则创建一个新实例并将其存储在 `instances` 中，否则直接返回存储在 `instances` 中的实例。这样就保证了该类只有一个实例。
"""
# 示例用法
instance1 = MyClass("Instance 1")
instance2 = MyClass("Instance 2")
print(instance1 is instance2)  # 输出 True
```
分布式系统中的单例任务是怎么实现的
```
通常需要考虑如何在多个节点或实例中保持任务的唯一性。
1. 使用分布式锁来确保只有一个节点或实例可以执行任务,可以使用基于 ZooKeeper、Redis 等实现的分布式锁服务，来保证任务的唯一性。
2. 使用 Leader Election 算法来选举一个节点或实例作为任务的执行者,选举出的领导者负责执行任务，其他节点或实例不执行任务。
3. 使用消息队列来确保任务只被消费一次,生产者将任务发送到消息队列中，消费者从消息队列中获取任务并执行。
4. 使用数据库中的标记来表示任务是否已经被执行,当任务执行时，将在数据库中标记任务状态，其他节点或实例在执行任务前先检查任务状态，避免重复执行。
```

### 工厂模式
工厂模式：包涵一个超类，这个超类提供一个抽象化的接口来创建一个特定类型的对象，而不是决定哪个对象可以被创建。

核心：让“生产”和“产品”解耦。
工厂模式的主要解决的问题：`将原来分布在各个地方的对象创建过程单独抽离出来，交给工厂类负责创建。其他地方想要使用对象直接找工厂（即调用工厂的方法）获取对象。`

优点：
```
一个调用者想创建一个对象，只要知道其名称就可以了。
扩展性高，如果想增加一个产品，只要扩展一个工厂类就可以。
屏蔽产品的具体实现，调用者只关心产品的接口。
```

```python
"""
用于创建对象的实例。工厂模式定义了一个创建对象的接口，但将对象的实际创建延迟到子类中。这样，客户端代码就不需要知道要实例化哪个具体类，只需要知道如何创建对象即可。工厂模式可以帮助解耦客户端代码和具体类的实现，提高代码的灵活性和可维护性。
"""
"""
简单工厂模式通过一个工厂类来实现对象的创建，客户端通过调用工厂类的静态方法或类方法来创建对象。
"""
class Car:
    def __init__(self, brand):
        self.brand = brand

class CarFactory:
    @staticmethod
    def create_car(brand):
        return Car(brand)

car = CarFactory.create_car("Toyota")

'工厂方法模式将工厂抽象为一个接口或基类，具体的工厂子类负责实现工厂接口并创建对象。'
from abc import ABC, abstractmethod

class Car(ABC):
    @abstractmethod
    def create_car(self, brand):
        pass

class ToyotaCar(Car):
    def create_car(self, brand):
        return Car(brand)

class HondaCar(Car):
    def create_car(self, brand):
        return Car(brand)

toyota_factory = ToyotaCar()
car = toyota_factory.create_car("Toyota")

'抽象工厂模式提供一个接口用于创建一系列相关或依赖对象的家族，而不需要指定具体类'
from abc import ABC, abstractmethod

class Car(ABC):
    @abstractmethod
    def create_engine(self):
        pass

    @abstractmethod
    def create_tire(self):
        pass

class ToyotaCar(Car):
    def create_engine(self):
        return "Toyota Engine"

    def create_tire(self):
        return "Toyota Tire"

class HondaCar(Car):
    def create_engine(self):
        return "Honda Engine"

    def create_tire(self):
        return "Honda Tire"

toyota_factory = ToyotaCar()
toyota_engine = toyota_factory.create_engine()
toyota_tire = toyota_factory.create_tire()

```

## 迭代器+生成器+装饰器+闭包
迭代器，生成器
```
迭代器: 是一个更抽象的概念，任何对象，如果它的类有 next 方法和 iter 方法返回自己本身，对于 string、list、dict、tuple 等这类容器对象，使用 for 循环遍历是很方便的。在后台 for 语句对容器对象调用 iter()函数，iter()是 python 的内置函数。iter()会返回一个定义了 next()方法的迭代器对象，它在容器中逐个访问容器内元素，next()也是 python 的内置函数。在没有后续元素时，next()会抛出一个 StopIteration 异常。

生成器（Generator）是创建迭代器的简单而强大的工具。它们写起来就像是正规的函数，只是在需要返回数据的时候使用 yield 语句。每次 next()被调用时，生成器会返回它脱离的位置（它记忆语句最后一次执行的位置和所有的数据值）

区别：生成器能做到迭代器能做的所有事,而且因为自动创建了 iter()和 next()方法,生成器显得特别简洁,而且生成器也是高效的，使用生成器表达式取代列表解析可以同时节省内存。除了创建和保存程序状态的自动方法,当发生器终结时,还会自动抛出 StopIteration 异常。
```
yield用法
```
yield 就是保存当前程序执行状态。你用 for 循环的时候，每次取一个元素的时候就会计算一次。用yield 的函数叫 generator，和 iterator 一样，它的好处是不用一次计算所有元素，而是用一次算一次，可以节省很多空间。generator每次计算需要上一次计算结果，所以用 yield，否则一 return，上次计算结果就没了。

定义生成器必须要使用 yield 这个关键词，yield 翻译成中文有「生产」这方面的意思。在 Python 中，它作为一个关键词，是生成器的标志。
```

迭代器和生成器
```python
# 可迭代对象: 列表、字典、集合等
# 迭代器示例
class MyIterator:
    def __init__(self, data):
        self.index = 0
        self.data = data

    def __iter__(self): # 创建
        return self

    def __next__(self): # 返回下一个元素
        if self.index >= len(self.data):
            raise StopIteration
        value = self.data[self.index]
        self.index += 1
        return value

"""
生成器是一种特殊的迭代器，它可以通过函数来创建。生成器函数使用 `yield` 关键字来产生值，每次调用生成器函数时，会从上一次 `yield` 语句处恢复执行。
"""
# 生成器示例
def my_generator(data):
    for item in data:
        yield item

# 使用迭代器
my_iter = MyIterator([1, 2, 3, 4, 5])
my_gen = my_generator([1, 2, 3, 4, 5])
```
调用函数得到生成器
```python
def f():
	yield 0
	yield 1
	yield 2

f # <function f at 0x00000000004EC1E0>
fa = f() # <generator object f at 0x0000000001DF1660>
>>> dir(fa)
['__class__', '__del__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__lt__', '__name__', '__ne__',
'__new__', '__next__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'close', 'gi_code', 'gi_frame', 'gi_running', 'gi_yieldfrom', 'send', 'throw']
# 在上面我们看到了iter() 和next()，虽然我们在函数体内仅仅是写了 yield，但它就已经是「迭代器」了
fa.__next__() # 0 生成器开始执行，遇到了第一个 yield，然后返回后面的 0，并且挂起（即暂停执行）。
fa.__next__() # 1 从上次暂停的位置开始，继续向下执行，遇到第二个 yield，返回后面的值 1，再挂起。
fa.__next__() # 2
fa.__next__()  # 从上次暂停的位置开始，继续向下执行，但是后面已经没有 yield 了，所以 **next**() 发生异常。
Traceback (most recent call last): File "<stdin>", line 1, in <module> StopIteration
```
### 装饰器
装饰器（Decorator）是 Python 中一种用于修改或扩展函数或类功能的工具，它可以在不改变原函数或类代码的情况下，对其进行功能增强或修改。
装饰器是一种特殊的闭包，就是在闭包的基础上传递了一个函数，然后覆盖原来函数的执行入口，以后调用这个函数的时候，就可以额外实现一些功能了。
通过使用 `@` 符号和装饰器函数的方式，可以很方便地将装饰器应用到目标函数或类上。
```python
import time
# timer装饰器函数接受一个函数作为参数，并返回一个新的函数 wrapper
def timer(func):
	# wrapper函数内部记录了函数执行的开始时间和结束时间，并输出函数执行时间。
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"Function {func.__name__} executed in {end_time - start_time} seconds")
        return result
    return wrapper

# 通过@timer的方式将time 装饰器应用到my_function上，从而实现了函数执行时间的输出。
@timer
def my_function():
    time.sleep(1)
    print("Function executed")

my_function()

```
装饰器中带变量和不带变量有什么区别
- 带变量的装饰器可以接受参数，而不带变量的装饰器不能接受参数。
```python
"""
不带：它接受一个函数作为参数，并返回一个新的函数 `wrapper`。在 `wrapper` 函数内部，对被装饰的函数进行了装饰处理。
"""
def simple_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before function execution")
        result = func(*args, **kwargs)
        print("After function execution")
        return result
    return wrapper

@simple_decorator
def my_function():
    print("Function executed")

my_function()
```

```python
"""
带：decorator_with_args是一个带变量的装饰器工厂函数，它接受一个参数 arg，并返回一个真正的装饰器函数 decorator。
在 decorator 函数内部，接受被装饰函数作为参数，并返回一个新的函数 wrapper。在 wrapper函数内部，对被装饰的函数进行了装饰处理，并可以使用传入的参数 arg。
"""
def decorator_with_args(arg):
    def decorator(func):
        def wrapper(*args, **kwargs):
            print(f"Decorator argument: {arg}")
            result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@decorator_with_args("arg_value")
def my_function():
    print("Function executed")

my_function()
```

装饰器：函数可以赋值给变量，函数可嵌套，函数对象可以作为另一个函数的参数。
装饰器本质上是一个 Python 函数，它可以在让其他函数在不需要做任何代码的变动的前提下增加额外的功能。装饰器的返回值也是一个函数的对象，它经常用于有切面需求的场景。 比如：插入日志、性能测试、事务处理、缓存、权限的校验等场景 有了装饰器就可以抽离出大量的与函数功能本身无关的雷同代码并发并继续使用。
函数可以作为参数传递的语言，可以使用装饰器
```python
def first(fun):
    def second():
        print('start')
        fun()
        print('end')
        print fun.__name__
    return second

def man():
    print('i am a man()')

f = first(man) # first函数接收了man函数作为参数，并将man函数以一个新的函数进行替换。
f()
# 明目张胆使用first函数封装man函数
# 执行结果
start
i am a man()
end
man
```
上述改成装饰器
```python
def first(func):
    def second():
        print('start')
        func()
        print('end')
        print (func.__name__)
    return second

# 用语法糖来封装man函数@[装饰器名称]
@first 
def man():
    print('i am a man()')

man()
```
装饰器的高级例子
```python
def check_admin(username):
    if username != 'admin':
        raise Exception('This user do not have permission')
'''
实现了一个特殊的栈，特殊在多了检查当前用户是否为 admin 这步判断，如果当前用户不是 admin，则抛出异常。上面的代码中将检查当前用户的身份写成了一个独立的函数 check_admin，在 push 和 pop 中只需要调用这个函数即可。
'''
class Stack:
    def __init__(self):
        self.item = []

    def push(self,username,item):
        check_admin(username=username) # 检查
        self.item.append(item)

    def pop(self,username):
        check_admin(username=username)
        if not self.item:
            raise Exception('NO elem in stack')
        return self.item.pop()


'''
装饰器效果：
而使用装饰器的时候，我们上来看到的就是对栈的操作语句，至于 check_admin 完全不会干扰到我们对当前函数的理解，所以使用了装饰器可读性更好了一些。它写完以后是很难调试的，并且使用「装饰器」的程序的速度会比不使用装饰器的程序更慢，所以还是要具体场景具体看待。
'''
def check_admin(func):
    def wrapper(*args, **kwargs):
        if kwargs.get('username') != 'admin':
            raise Exception('This user do not have permission')
        return func(*args, **kwargs)
    return wrapper

class Stack:
    def __init__(self):
        self.item = []

    @check_admin
    def push(self,username,item):
        self.item.append(item)

    @check_admin
    def pop(self,username):
        if not self.item:
            raise Exception('NO elem in stack')
        return self.item.pop()

```
### 闭包
闭包是一个可以由另一个函数动态生成的函数，并且可以改变和存储函数外创建的变量的值。
如果在一个内部函数里，对在外部作用域（但不是在全局作用域）的变量进行引用，那么内部函数就被认为是闭包（closure）。
闭包特点：必须有一个内嵌函数；内嵌函数必须引用外部函数中的变量；外部函数的返回值必须是内嵌函数。

```
a = 1
def fun():
	print(a)
...
fun() = 1
可以运行这个代码

def fun1():
	b = 1

print(b)
Traceback (most recent call last):
 File "<stdin>", line 1, in <module>
NameError: name 'b' is not defined

在函数 fun() 里可以直接使用外面的 a = 1，但是在函数 fun1() 外面不能使用它里面所定义的 b = 1

>>> def fun():
...    a = 1
...    def fun1():  # 闭包
...            return a
...    return fun1
...
>>> f = fun()
>>> print(f())
1
闭包实际上就是一个函数，但是这个函数要具有 1.定义在另外一个函数里面(嵌套函数)；2.引用其所在环境的自由变量。
上述例子通过闭包在 fun() 执行完毕时，a = 1依然可以在 f() 中，即 fun1() 函数中存在，并没有被收回，所以 print(f()) 才得到了结果。
```
用闭包方法写一元二次方程
```python
def fun(a,b,c):
	def para(x):
		return a*x**2 + b*x + c
		return para

f = fun(1,2,3) # 定义了一个一元二次函数的函数对象
print(f(2)) = 11
```
猴子补丁:在运行期间动态修改一个类或模块。

```python
class A:
    def func(self):
        print("Hi")
        
def monkey(self):
	print "Hi, monkey"

m.A.func = monkey
a = m.A()
a.func()

output: Hi, Monkey
```
猴子补丁主要有以下几个用处：
```
在运行时替换方法、属性等；
在不修改第三方代码的情况下增加原来不支持的功能；
在运行时为内存中的对象增加 patch 而不是在磁盘的源代码中增加
```

```python
# 装饰器方式
import time

def log_func_time(func):
	def wrapper():
		start_time = time.perf_counter()
		my_func = func()
		end_time = time.perf_counter()
		print('method {} spend {} ms.'.format(func.__name__, (end_time-start_time)*1000))
		return my_func
	return wrapper

@log_func_time
def_calculate_func1():
	list1 = [i for i in range(100000)]
	
@log_func_time
def_calculate_func2():
	list1 = (i for i in range(100000)) # Generator

def_calculate_func1() # 封装的个数就是100000个
def_calculate_func2() # 消耗的时间远远低于1，生成列表，记录一定的算法规则，生成器，需要用到的时候再调用。根据一定的规律算法生成
```

yield：一个函数里面如果被定义了yield，那么这个函数就是一个生成器。用next()使用生成器。从第一个yield开始，遇到第一个yield跳出函数。继续next()直到没有yield后StopIteration
return：返回结果并且终止函数
```python
def foo():
	yield 1
	yield 2
	...
def f():
	for i in range(10):
		yield i*2
f = foo()
for i in f:
	print(i)
	
```
