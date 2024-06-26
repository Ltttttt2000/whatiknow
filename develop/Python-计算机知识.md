## 一、 进程+线程+协程

### 进程
```
1. 操作系统进行资源分配和调度的基本单位，多个进程之间相互独立
2. 稳定性好，如果一个进程崩溃，不影响其他进程
3. 进程资源消耗大，开启的进程数量有限制
```
Python多进程之间如何通信：队列、管道、共享内存、信号量
```python
"""
1. 队列Queue
可以使用 multiprocessing.Queue 来创建一个进程间共享的队列，进程可以通过put()和get()方法向队列中放入和获取数据。
"""
from multiprocessing import Process, Queue
def worker(queue):
    data = queue.get()
    # 处理数据
    print(data)

if __name__ == "__main__":
    queue = Queue()
    p = Process(target=worker, args=(queue,))
    p.start()
    queue.put("Hello, world!")
    p.join()


"""
2. 管道Pipe
multiprocessing.Pipe来创建管道，通过 send() 和 recv() 方法在进程之间传递数据。
"""
from multiprocessing import Process, Pipe
def worker(conn):
    data = conn.recv()
    # 处理数据
    print(data)

if __name__ == "__main__":
    parent_conn, child_conn = Pipe()
    p = Process(target=worker, args=(child_conn,))
    p.start()
    parent_conn.send("Hello, world!")
    p.join()

"""
3. 共享内存shared memory
允许多个进程访问同一块内存区域，可以使用 `multiprocessing.Value` 和 `multiprocessing.Array` 来创建共享内存。
"""
from multiprocessing import Process, Value, Array
def worker(val, arr):
    val.value = 1
    arr[0] = 2

if __name__ == "__main__":
    v = Value('i', 0)
    a = Array('i', range(5))
    p = Process(target=worker, args=(v, a))
    p.start()
    p.join()
    print(v.value, a[:])


"""
4. 信号量semaphore
信号量用于控制多个进程对共享资源的访问，可以使用 `multiprocessing.Semaphore` 来创建信号量。
"""
from multiprocessing import Process, Semaphore
def worker(sem):
    sem.acquire()
    # 访问共享资源
    sem.release()

if __name__ == "__main__":
    sem = Semaphore(1)
    p1 = Process(target=worker, args=(sem,))
    p2 = Process(target=worker, args=(sem,))
    p1.start()
    p2.start()
    p1.join()
    p2.join()
```
### 线程
```
1. CPU进行资源分配和调度的基本单位，线程是进程的一部分，是比进程更小的能独立运行的基本单位，一个进程下的多个线程可以共享改进程的所有资源。
2. 如果IO操作密集，则可以多线程运行效率高，缺点是如果一个线程崩溃都会造成进程的崩溃
```

如何在Python中实现多线程
```python
"""
CPU密集型任务中无法获得性能上的提升，因为Python GIL会限制同一时刻只能由一个线程执行。然而对于IO密集性，多线程可以带来性能上提升。
"""
import threading
# 定义一个简单的线程函数
def thread_function(name):
    print(f"Thread {name} is running")
# 创建并启动多个线程
for i in range(5):
    t = threading.Thread(target=thread_function, args=(i,))
    t.start()
```
### 协程Coroutine 
```
协程是一种轻量级的线程. 可以在单个线程中实现多个任务的并发执行，从而提高程序的效率。协程可以在执行过程中暂停，并在需要时恢复执行，这种特性使得协程非常适合于 I/O 密集型任务。

协程的作用是在执行函数A时可以随时中断去执行函数B，然后中断函数B继续执行函数A（可以自由切换）。但这一过程并不是函数调用，这一整个过程看似像多线程，然而协程只有一个线程执行。
```
协程的优点
```
极高的执行效率。
	- 因为子程序切换不是线程切换，而是由程序自身控制，因此，没有线程切换的开销，和多线程比，线程数量越多，协程的性能优势就越明显。
	- 不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了，所以执行效率比多线程高很多。
```
因为协程是一个线程执行，那怎么利用多核CPU呢？
```
最简单的方法是多进程+协程，既充分利用多核，又充分发挥协程的高效率，可获得极高的性能。
```
其他一些重要的点：

协程并没有增加线程数量，只是在线程的基础之上通过**分时复用**的方式运行多个协程，而且协程的切换在用户态完成，切换的代价比线程从用户态到内核态的代价小很多。

因此在协程调用阻塞IO操作的时候，操作系统会让线程进入阻塞状态，当前的协程和其它绑定在该线程之上的协程都会陷入阻塞而得不到调度，这往往是不能接受的。

因此在协程中不能调用导致线程阻塞的操作。也就是说，协程只有和**异步IO**结合起来，才能发挥最大的威力。
协程对计算密集型的任务没有太大的好处，计算密集型的任务本身不需要大量的线程切换，因为协程主要解决以往线程或者进程上下文切换的开销问题，所以协程主要对那些I/O密集型应用更好。
### 应用
多进程和多线程
```
IO密集的:用多线程，在用户输入，Sleep的时候，可以切换到其他进程执行，减少等待的时间；
CPU密集的:用多进程，因为如果IO操作少，用多线程的话，因为线程共享一个全局解释器锁，当前运行的线程会霸占GIL，其他线程没有GIL，就不能充分利用多核CPU的优势。
```
进程切换过程中会发生什么，线程切换过程中会发生什么
```
在操作系统中，进程和线程的切换都是通过操作系统内核来完成的，具体的实现方式会有所不同，但一般都包括以下几个主要步骤：

1. 保存上下文：保存上下文信息，包括程序计数器（PC）、堆栈指针（SP）、寄存器内容等。这些信息会保存到内存中的一个数据结构中，以便后续恢复。
2. 切换内核栈：进程或线程的内核栈是用来执行内核代码时保存局部变量和临时数据的地方。在切换进程或线程时，操作系统会切换到相应的内核栈，以便执行必要的内核代码。
3. 切换页表：进程或线程的地址空间是由操作系统管理的，包括虚拟地址到物理地址的映射关系。在切换进程或线程时，操作系统会切换到相应的页表，以便访问正确的物理内存。
4. 恢复上下文：在切换到新的进程或线程之后，操作系统会从保存的上下文信息中恢复新进程或线程的状态，包括程序计数器、堆栈指针、寄存器内容等。
5. 更新运行队列：操作系统会更新进程或线程的运行队列，将新的进程或线程添加到可运行队列中，并从中选择下一个要执行的进程或线程。

在进程切换过程中，由于涉及到地址空间的切换等复杂操作，因此切换的开销比较大，通常需要几毫秒甚至更长时间。而在线程切换过程中，由于线程共享同一地址空间，因此切换的开销通常比进程切换小很多，一般只需要几微秒。
```
Linux进程崩溃后如何排查（假设该进程没有记录日志文件）
```
1. 核心转储文件（Core Dump）：如果系统配置了核心转储，可以通过查看核心转储文件来了解进程崩溃时的状态。核心转储文件通常在进程的工作目录或 `/var/crash` 目录下。可以使用 `gdb` 工具加载核心转储文件，并查看堆栈信息和变量值等来分析问题。
2. 进程状态检查：
使用 `ps` 命令查看进程状态，可以了解进程是否正在运行或已经停止。如果进程没有退出，但是出现了异常行为，可以尝试使用 `strace` 或`ltrace` 命令来跟踪系统调用或库调用，以确定问题所在。
3. 系统日志：查看系统日志（如 `/var/log/messages`、`/var/log/syslog` 等）可以获取系统和进程的运行信息，可能会包含有关进程崩溃的相关信息。
4. 内存和资源使用情况：使用 `top` 或 `htop` 命令查看系统的内存和 CPU 使用情况，以确定是否存在资源耗尽导致进程崩溃的情况。
5. 重启进程：如果进程是可以重启的，并且崩溃是偶发性的，可以尝试重启进程并观察是否会再次崩溃，以便进一步排查问题。
```
多线程交互，访问数据，如果访问到了就不访问了，怎么避免重读？
```
创建一个已访问数据列表，用于存储已经访问过的数据，并加上互斥锁，在多线程访问数据的时候先查看数据是否已经在已访问的列表中，若已存在就直接跳过。
```
多进程适合在 CPU 密集型操作(cpu 操作指令比较多，如位数多的浮点运算)。
多线程适合在 IO 密集型操作(读写数据操作较多的，比如爬虫)。
并行（parallel）是指同一时刻，两个或两个以上时间同时发生。（逻辑运算）
并发（parallel）是指同一时间间隔（同一段时间），两个或两个以上时间同时发生。（大量IO操作）
## 二、Cookie和Session
HTTP是无连接无状态的（这次访问服务器，服务器不会记住）
怎么记住/保持登陆状态：存储
让浏览器记住用户名和密码
实现每次HTTP请求都自动带数据给服务器的技术-->Cookie
```
Cookie
浏览器发出HTTP请求
服务器进行Cookie设置（set-cookie）有名和值两个属性
服务器填充完整，cookie发给浏览器后浏览器会保存
浏览器以后发送的请求都自带cookie（名+值）
Cookie就是一种存储在浏览器的数据，很不安全！
用途：跟踪用户状态、存储偏好设置、记录访问历史

Session
用途：跟踪会话状态、存储用户登陆信息
会话开始：浏览器访问服务器
不同的网站对于每个用户的会话都设定了时间（结束会话的时间）以及唯一的ID session id。保存在数据库里
用户名和密码发送给服务器，服务器set-cookie（session ID+时间）
发给浏览器，浏览器保存的是session id（无规律的字符串）
（发送前会进行签名，如果修改了sessionid就无法识别）
每个请求都会自动发送cookie(session id)到相应服务器，知道有效期失效后，需要重新输入

特定时间大量用户访问时需要存储大量session ID，会出现超载，需要分配一些用户到其他服务器，其他服务器需要通用的session ID，数据库也怕崩溃，于是出现了JWT技术（Json Web Token)
Token
JWT签名密文，浏览器以cookie形式保存，存储在用户端
header.payload.signature
```
Cookie和Session的区别
```
1. session在服务器端，cookie在客户端（浏览器）
2. session的运行依赖session id，而session ID是存在cookie中的，也就是说，如果浏览器禁用了cookie，同时session也会失效，存储Session时，键与cookie中的session ID相同，值是开发人员设置的键值对信息，进行了base64编码，过期时间由开发人员设置。
3. cookie安全性比session差
4. 
```
Cookie的生命周期是永久的吗
```
不是所有的 Cookie 的生命周期都是永久的。Cookie 的生命周期可以通过设置其过期时间来控制。具体而言，有两种类型的 Cookie：

1. 会话 Cookie：这种 Cookie 的生命周期仅限于用户的会话期间，也就是用户在浏览器中打开并使用网站时。一旦用户关闭浏览器，这些 Cookie 就会被删除。
    
2. 持久 Cookie：这种 Cookie 有一个指定的过期时间，在这之前会一直保留在用户的计算机上。持久 Cookie 可以用来存储一些长期的用户偏好或身份验证信息，以便用户下次访问网站时自动登录或者保留特定设置。
```
Session的生存周期在一次会话后关闭，那刷新浏览器会怎样？
```
在使用会话（Session）Cookie时，通常情况下，当用户关闭浏览器时，会话将被标记为结束，服务器将删除与该会话相关的所有数据，包括Session Cookie。这意味着用户重新打开浏览器时，将开始一个新的会话，并且之前存储在会话中的任何数据都将丢失。

然而，如果用户在会话期间刷新浏览器，会话并不会结束，而是继续。这是因为刷新浏览器并不会导致浏览器关闭，所以会话仍然有效。在这种情况下，会话Cookie仍然存在，并且服务器仍然可以使用会话ID来识别和验证用户。
```
Cookie、Session、Token的区别以及应用场景
```
1. Cookie：
    - 由服务器发送到用户浏览器并存储在用户本地计算机上的小型文本文件。它通常包含有关用户和网站的信息，如用户的身份认证令牌、偏好设置等。Cookie 通常用于存储用户的身份验证状态、语言偏好、购物车内容等，以便在用户访问同一网站时保持这些状态。
2. Session：
	- 在服务器端存储的关于用户会话的信息。当用户首次访问服务器时，服务器将为用户创建一个唯一的会话 ID，并将该 ID 存储在 Cookie 中。服务器使用该会话 ID 来存储和检索与用户会话相关的数据。通常用于存储用户的登录状态、购物车内容等需要在不同页面之间共享的数据。
3. Token：
    - 一种用于身份验证的令牌，通常是一个长字符串，由服务器生成并发送给客户端，客户端将其存储并在以后的请求中发送给服务器以验证身份。通常用于实现基于令牌的身份验证（Token-based Authentication），例如 JSON Web Token（JWT）等，它们可以在客户端和服务器之间安全地传输信息，而不必存储会话状态。
```

## 三、 Python解释器
Python解释器的工作原理
```
Python 解释器是将 Python 代码转换为机器语言并执行的程序。

1. 词法分析（Lexical Analysis）：将代码分解为一系列标记（tokens），包括关键字、标识符、运算符等。
2. 语法分析（Syntax Analysis）：将标记转换为语法树（Syntax Tree），以便后续执行。
3. 编译（Compilation）：将语法树转换为字节码（Bytecode），字节码是一种与特定平台无关的中间代码。
4. 执行（Execution）：最后，解释器将字节码传递给 Python 虚拟机（Python Virtual Machine，简称为 Python VM），Python VM 在解释器中执行字节码，并将结果返回给用户。

Python 是一种解释型语言，因此每次执行都需要通过解释器将代码转换为机器语言，这与编译型语言不同。
```
GIL：全局解释器锁
```
全局解释器锁（Global Interpreter Lock，GIL）是 Python 解释器中的一种机制，用于保护共享数据结构（如对象引用计数）免受多线程并发访问的影响。
GIL 的存在是由于 Python 的内存管理机制（主要是引用计数）在多线程环境下存在线程安全问题，因此需要一种机制来确保共享数据结构的安全访问。

同一进程中假设有多个线程运行，一个线程运行时会霸占Python解释器，即加了一把锁GIL，使该进程的其他线程无法运行。如果线程运行过程中遇到耗时操作，则解释器锁解开，使其他线程运行。所以多线程中，线程的运行依然是有先后顺序的，不是同时进行。
多进程中因为每个进程都能被系统分配资源，相当于每个进程有了一个Python解释器，所以多进程可以实现多个进程同时运行，缺点是进程系统资源开销大。
```
为什么要设置GIL而不是允许开发者自己设置线程锁
```
1. 简化内存管理：Python 使用简单的引用计数内存管理方式，它不需要复杂的垃圾回收算法。GIL 可以确保在增加或减少对象引用计数时的线程安全性，从而简化了内存管理的实现。
2. 保证解释器的线程安全性：由于 Python 解释器本身并不是线程安全的，多个线程同时执行 Python 代码可能会导致解释器内部状态的混乱。GIL 可以确保在任何时刻只有一个线程执行 Python 字节码，从而保证解释器的线程安全性。
3. 避免 C 扩展的线程安全问题：Python 中的很多模块是使用 C 语言编写的，这些 C 扩展可能并不是线程安全的。GIL 可以确保在执行 C 扩展时只有一个线程在执行，从而避免了 C 扩展的线程安全问题。

虽然 GIL 可以确保共享数据结构的线程安全性，但也带来了一些问题，如限制了多核 CPU 的利用率，导致 Python 在 CPU 密集型任务上的性能表现不佳。因此，Python 社区也在不断探索去除或优化 GIL 的方法，以提高 Python 在多核环境下的性能。
```

乐观锁和悲观锁
```
悲观锁：每次拿数据的时候都认为别人会修改，所以每次都会上锁，这样别人想拿这个数据就会直接block直到它拿到锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，在操作前先上锁。
乐观锁：每次拿数据的时候都认为别人不会修改，所以不会上锁，但更新的时候会判断一下在此期间有没有人更新这个数据，可以用版本号机制，乐观锁适用于多读的应用类型，可以提高吞吐量。
```
如何保证分布式锁唯一，在不适用存储中间件的情况秀下如何实现分布式锁
```
在不使用存储中间件的情况下，可以考虑使用基于时间戳（Timestamp）的方式来实现简单的分布式锁。具体实现方法如下：
1. 获取锁：每个节点尝试获取锁时，都记录当前时间戳作为锁的持有者和持有时间。
2. 释放锁：当持有锁的节点释放锁时，将持有时间设置为一个较小的值（例如 0），表示释放锁。 
3. 判断锁是否可用：其他节点在尝试获取锁时，先检查当前时间戳与持有者的时间戳的差值是否大于某个阈值（例如锁的超时时间）。如果大于阈值，则认为锁已经释放，可以获取锁。
    
这种方式的实现比较简单，但是存在一些问题：
- 时间同步问题：由于分布式系统中节点的时间可能存在差异，因此可能会导致时间判断不准确。
- 死锁问题：如果持有锁的节点在释放锁之前发生故障，可能会导致死锁情况。

综上所述，虽然可以通过基于时间戳的方式实现简单的分布式锁，但是在实际应用中，推荐使用存储中间件（如 Redis、ZooKeeper 等）来实现分布式锁，以确保锁的可靠性和稳定性。
```

## 四、 Python内存管理

如何管理内存
```
python的内存管理是由私有heap空间管理的。所有的python对象和数据结构都在一个私有heap中，程序员没有访问该heap的权限，只有解释器才能对它进行操作。为python的heap空间分配内存是由python的内存管理模块进行的，其核心API会提供一些访问该模块方法的方法供程序员使用。python有自带的垃圾回收系统，它回收并释放没有被使用的内存，让它们能够被其他程序使用。

Python是怎样管理内存的？

1）引用计数机制：Python内部使用引用计数，来保持追踪内存中的对象。

2）垃圾回收机制：当一个对象的引用计数归零时，它将被垃圾收集机制处理掉；循环垃圾回收器，确保释放循环引用对象。

3).内存池机制：

Python提供了对内存的垃圾收集机制，但是它将不用的内存放到内存池而不是返回给操作系统:

Pymalloc机制：为了加速Python的执行效率，Python引入了一个内存池机制，用于管理对小块内存的申请和释放。

对于Python对象，如整数，浮点数和List，都有其独立的私有内存池，对象间不共享他们的内存池。也就是说如果你分配又释了大量的整数，用于缓存这些整数的内存就不能再分配给浮点数。
```
每当Python退出时，为什么不取消分配所有内存？
```
1. 每当Python退出时，尤其是那些循环引用其他对象或从全局名称空间引用的对象的Python模块都不会总是被取消分配或释放。
2. 不能取消分配C库保留的那些内存部分。
3. 退出时，由于具有自己有效的清除机制，Python会尝试取消分配/销毁所有其他对象。
```
垃圾回收机制
```
Python 的垃圾回收机制主要基于引用计数和循环引用检测两种方式：
1. 引用计数：使用引用计数来跟踪对象的引用数量。当一个对象被创建时，它的引用计数为1，当一个对象被引用时，它的引用计数加1，当一个对象不再被引用时，它的引用计数减1。当对象的引用计数变为0时，说明没有任何引用指向该对象，Python 就会回收该对象的内存。
2. 循环引用检测：引用计数虽然简单高效，但存在无法解决循环引用的问题。为了解决这个问题，Python 使用了一种基于标记-清除（mark and sweep）算法的循环引用检测机制。当引用计数无法回收对象时，Python 会使用标记-清除算法来检测循环引用，并清除不可达的对象。

在标记-清除算法中，Python 会从根对象（如全局变量、函数栈等）开始遍历所有可达对象，并将它们标记为活动对象。然后，Python 会遍历所有对象，清除未标记的对象，最终回收这些未标记对象的内存。

Python 的垃圾回收机制是自动运行的，开发者无需手动管理内存。Python 的解释器会根据需要在适当的时候自动进行垃圾回收，以释放不再使用的内存空间。
```
Python的内存管理
```
python中的内存管理由Python私有堆空间管理。所有Python对象和数据结构都位于私有堆中。程序员无权访问此私有堆。python解释器负责处理这个问题。
Python对象的堆空间分配由Python的内存管理器完成。核心API提供了一些程序员编写代码的工具。
Python还有一个内置的垃圾收集器，它可以回收所有未使用的内存，并使其可用于堆空间。

内存管理机制：引用计数、垃圾回收、内存池
引用计数：引用计数是一种非常高效的内存管理手段， 当一个 Python 对象被引用时其引用计数增加 1， 当其不再被一个变量引用时则计数减 1. 当引用计数等于 0 时对象被删除。

垃圾回收：
	(1) 引用计数
	引用计数也是一种垃圾收集机制，当 Python 的某个对象的引用计数降为 0 时，说明没有任何引用指向该对象，该对象就成为要被回收的垃圾了。比如某个新建对象，它被分配给某个引用，对象的引用计数变为 1。如果引用被删除，对象的引用计数为 0，那么该对象就可以被垃圾回收。不过如果出现循环引用的话，引用计数机制就不再起有效的作用了。
	(2)标记清除
	如果两个对象的引用计数都为 1，但是仅仅存在他们之间的循环引用，那么这两个对象都是需要被
回收的，也就是说，它们的引用计数虽然表现为非 0，但实际上有效的引用计数为 0。所以先将循环引
用摘掉，就会得出这两个对象的有效计数。
	(3) 分代回收
	从前面“标记-清除”这样的垃圾收集机制来看，这种垃圾收集机制所带来的额外操作实际上与系统
中总的内存块的数量是相关的，当需要回收的内存块越多时，垃圾检测带来的额外操作就越多，而垃圾
回收带来的额外操作就越少；反之，当需回收的内存块越少时，垃圾检测就将比垃圾回收带来更少的额
外操作。
	e.g. 当某些内存块 M 经过了 3 次垃圾收集的清洗之后还存活时，我们就将内存块 M 划到一个集合 A 中去，而新分配的内存都划分到集合 B 中去。当垃圾收集开始工作时，大多数情况都只对集合 B 进行垃圾回收，而对集合 A 进行垃圾回收要隔相当长一段时间后才进行，这就使得垃圾收集机制需要处理的内存少了，效率自然就提高了。在这个过程中，集合 B 中的某些内存块由于存活时间长而会被转移到集合 A 中，当然，集合 A 中实际上也存在一些垃圾，这些垃圾的回收会因为这种分代的机制而被延迟。

内存池：
	(1) Python 的内存机制呈现金字塔形状，-1，-2 层主要有操作系统进行操作
	(2) 第 0 层是 C 中的 malloc，free 等内存分配和释放函数进行操作
	(3)第 1 层和第 2 层是内存池，有 Python 的接口函数 PyMem_Malloc 函数实现，当对象小于256K 时有该层直接分配内存
	(4) 第 3 层是最上层，也就是我们对 Python 对象的直接操作
Python 在运行期间会大量地执行 malloc 和 free 的操作，频繁地在用户态和核心态之间进行切换，这将严重影响 Python 的执行效率。为了加速 Python 的执行效率，Python 引入了一个内存池机制，用于管理对小块内存的申请和释放。
Python 内部默认的小块内存与大块内存的分界点定在 256 个字节，当申请的内存小于 256 字节时，PyObject_Malloc 会在内存池中申请内存；当申请的内存大于 256 字节时PyObject_Malloc 的行为将蜕化为 malloc 的行为。当然，通过修改 Python 源代码，我们可以改变这个默认值，从而改变 Python 的默认内存管理行为。
```
内存泄露和如何避免
```
内存泄露：由于疏忽或错误造成程序未能释放已经不再使用的内存的情况。
内存泄漏并非指内存在物理上的消失，而是应用程序分配某段内存后，由于设计错误，失去了对该段内存的控制，因而造成了内存的浪费。导致程序运行速度减慢甚至系统崩溃等严重后果。

del() 函数的对象间的循环引用是导致内存泄漏的主凶。
不使用一个对象时使用:del object 来删除一个对象的引用计数就可以有效防止内存泄漏问题。
通过 Python 扩展模块 gc 来查看不能回收的对象的详细信息。
可以通过 sys.getrefcount(obj) 来获取对象的引用计数，并根据返回值是否为 0 来判断是否内存泄漏。

```
当退出Python时是否释放全部内存
```
不是！循环引用其它对象或引用自全局命名空间的对象的模块，在Python退出时并非完全释放。另外，也不会释放C库保留的内存部分。
```
