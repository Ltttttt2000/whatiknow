JDK：Development Kit 开发工具
JRE：runtime environment 核心库
JVM：虚拟机（Java可跨平台）
JDK>JRE>JVM

Java语言特点：
1. 面向对象（封装，继承，多态）
2. 平台无关性、健壮性（强类型，异常处理，垃圾回收）
3. 支持网络编程、多线程
4. 安全性

## 面向对象
| 面向过程 | 面向对象 |
| ---- | ---- |
| 具体化、流程化 | 模型化，把面向过程抽象成类 |
| 性能高（单片机，Linux） | 实例化开销大、消耗资源 |
| 不易维护 | 易维护，复用，扩展，低耦合 |
封装：把对象的属性私有化，对外有公共访问；便于使用，提高复用性和安全性
继承：不能选择性继承父类，子类有父类private的属性和方法，可对父类扩展，可重写
多态：父类/接口定义的引用变量可指向子类或具体实现类的实例对象，提高扩展性，继承or接口

overload：编译时多态，前绑定；同一个类中参数不同，与返回值修饰符无关
override：运行时多态，后绑定；不同类父子类中方法名参数相同。返回值<=父类（异常）；访问修饰>=父类（private父类不能）
多态实现的必要条件：继承、重写、向上转型

面向对象的五大原则
```
1. 单一职责Signle Responsibility Princle (SRP) 类的功能要单一
2. 开放封闭 Open-Close (OCP) 一个模块扩展时开发，对修改封闭
3. 里式替换 the Liskov Substitution (LSP) 子类可替换父类出现的地方
4. 依赖倒置 Dependency Inversion (DIP) 高层次不依赖低层模块，抽象不依赖具体实现
5. 接口分离 Interface Segregation(ISP) 多个接口比一个通用的好
```

## 基本类型
数据类型和所占字节数
基本类型：byte(1), short(2), int(4), long(8), float(4), double(8), char(2), boolean(1)
引用类型：class，interface，数组

switch: Java7以后可以用于byte, short, int, char, string枚举，不能用long

```java
Math.round(11.5) = 12
Math.round(-11.5) = -11
float f = 3.4 语法错误：3.4为双精度double, doublt->float窄化，有精度损失。
float f = (float)3.4 或 float f = 3.4F

short s1 = 1; s1 = s1 + 1 错误：1是int类型要强制转换成short再赋值运算
short s1 = 1; s1 += 1 正确：有隐含强制转换
```

访问修饰符

|  | 当前类 | 同包 | 子类 | 其他包 | 用于 |
| ---- | ---- | ---- | ---- | ---- | ---- |
| private | ✅ | ❌ | ❌ | ❌ | 变量、方法 |
| default | ✅ | ✅ | ❌ | ❌ | 类、接口、变量、方法 |
| protected | ✅ | ✅ | ✅ | ❌ | 变量、方法 |
| public | ✅ | ✅ | ✅ | ✅ | 类、接口、变量、方法 |

运算符
& 与 && 短路与，若左边为false，右边不会运算

关键字
goto：保留字，不使用
final, this, super, static
```
this: 指向对象本身的一个指针
		1. 普通直接引用
		2. 形参与成员名重名用this.age = age
		3. 引用本类构造函数this()

super: 指向父类
		1. 普通直接引用super.xx 引用父类成员
		2. 区别子类、父类重名的成员变量
		3. 引用父类构造函数super()
		4. this, super不出现在同一个construstor里，不再static环境中使用

static: 静态
		1. 创建独立于具体对象的域变量或者方法
		2. 没创建对象也能用属性和方法
		3. 形成静态代码块优化程序性能，类初次加载按序只执行一次
		4. static变量类加载时分配空间，以后创建类对象不重新分
		5. 修饰：成员变量、方法、静态代码块、内部类、静态导包
		6. 静态只能访问static，非静态都能访问

final: 修饰类、属性、方法
		1. final class不能被继承
		2. final method不被重写
		3. final parameter不能改变

finally: 作用try-catch中，不管有无异常都执行，关闭资源
finalize: object类的方法System.gc()时调用GC，是否可回收的判断

```

类与接口

|  | abstract class | interface |
| ---- | ---- | ---- |
| 相同点 | 都不能实例化，位于继承的前端 | 被其他继承，含抽象方法 |
| 定义 | 捕捉子类通用特性，模版设计 | 抽象方法的集合/行为规范 |
| 实现 | extends，若子类不抽象要提高所有方法的实现 | implements，提供方法实现 |
| 修饰符 | 任意 | 默认public，不能private,protected |
| constructor | 可以有 | 不能有 |
| 多继承 | 一个子类只能继承一个抽象类 | 可以实现多个 |
| 字段声明 | 任意 | 默认static, final |
选择：优先选接口interface，尽量少用abstract class。要定义子类的行为又要为子类提供通用功能时用抽象类
普通类不能有abstract method
抽象类不能直接实例化，不能用final（final不被继承）

对象new object
String B = new String("ab")
A对象（String）实例在堆内存，1个A可有n个B指向
B对象引用指向，栈内存 B->0/1个A
对象相等判断
\==  判断地址是否相等（是否同一个对象）
equals() 判断内容是否相等
hashcode获取哈希码，返回int，若两个对象相等，hashcode相同，但hashcode相同不一定相等。
值传递：传递对象到方法可改变，并返回

变量与方法
1. 成员变量：方法外部，类内部；针对整个类有效；堆内存，随object而死亡；有默认初始值；就近原则
2. 局部变量：类方法中的；只在某个范围有效；方法被用时在栈内存，用完释放；必须赋值；先局部范围找，然后成员位置找

若父类中只定义了constructor(paras..)而子类中没用super-> 编译错误
生成类对象自动执行，不用调用，名字与类相同
静态变量：属于类，只有一份；初始化按照定义顺序
实例对象：N个对象有N份

### 内部类
1. 成员内部类：所有类都可访问 instance.new Inner()
2. 局部内部类：定义在方法中，在方法内new class()
3. 匿名内部类：必须继承抽象类/接口，不能用static，形参用时为final
4. 静态内部类：可访问外部所有静态static class.. new外部类.静态内部类()

优点
```
1. 可访问创建它的外部类内容，包含私有数据
2. 不为同一包其他类所见：封装性好
3. 实现多重继承
4. 匿名方便定义回调
```

Java IO流

|  | 输入流 | 输出流 |
| ---- | ---- | ---- |
| 字节流 | InputStream | OutputStream |
| 字符流 | Reader | Writer |
BIO：同步阻塞，简单，并发处理能力低
NIO：同步非阻塞，多路复用，通过channel通讯
AIO：异步非阻塞，NIO2，基于事件和回调机制


## 常用API: String, Date, 包装类
==String：不是基础类型，是reference type!!==
字符串常量池->堆内存，提高内存使用率，避免相同String用多空间
创建String：JVM检查pool，若存在则返回引用，否则实例化
不变性：immutable只读，保证一致性, final->安全性
String利用了final修饰的char类型
多开辟内存改变
不能被继承
String str = 'i' （静态区）不等于 String str = new String('i')（堆上的对象）

| String | StringBuilder | StringBuffer |
| ---- | ---- | ---- |
| 不可变 | 继承AbstractStringBuilder类，可变 | 继承AbstractStringBuilder类，可变，用char[]保留 |
| 线程安全 | 非线程安全 | 有同步锁，线程安全 |
| 改变时new新对象 |  | 对象本身操作 |
| 少量数据 | 单线程大量 | 多线程大量数据 |

==Date (java.util)==
毫秒数转为日期对象 DataFormat->Calendar

==包装类相关==
1. 自动装箱：将基本类型用对应的引用类包装int->Integer
2. 自动拆箱：包装类->基本类
Wrapper Class

| 基本类 | boolean | char | byte | short | int | float | double |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 包装类 | Boolean | Character | Byte |  | Integer |  |  |