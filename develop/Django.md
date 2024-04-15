服务端python开发做什么
```
Web开发，Web爬虫和数据采集，数据分析处理等
```
创建项目的命令
```
django-admin startproject [projectname]
python manage.py startapp [appname]
```
创建项目后，项目文件夹下的组成部分（对MVT的理解）
```
项目文件夹下的组成部分：
	- manage.py 项目运行的入口，指定配置文件路径
	- 与项目同名的目录，包含项目的配置文件：
		 __init.py 空文件，可以被当作包使用
		 settings.py 项目的整体配置文件
		 urls.py URL配置文件
		 wsgi.py 项目与WSGI兼容的Web服务器
```
MVT/MVC
```
MVC
Model: 模型，与数据库进行交互
View: 视图，负责产生HTML页面
Controller: 控制器，接收请求，进行处理，与M和V进行交互，返回应答
流程：
	1. 用户点击注按钮，将要注册的信息发送给网站服务器。
	2. Controller 控制器接收到用户的注册信息，Controller 会告诉 Model 层将用户的注册信息保存到数据库
	3. Model 层将用户的注册信息保存到数据库
	4. 数据保存之后将保存的结果返回给 Model 模型
	5. Model 层将保存的结果返回给 Controller 控制器
	6. Controller 控制器收到保存的结果之后，或告诉 View 视图，view 视图产生一个 html 页面
	7. View 将产生的 Html 页面的内容给了 Controller 控制器
	8. Controller 将 Html 页面的内容返回给浏览器
	9. 浏览器接受到服务器 Controller 返回的 Html 页面进行解析展示

MVT
- Model: 模型，与数据库进行交互.  用于定义数据结构的部分，它们被定义为Python类，并通过ORM系统映射到数据库中。模型定义了数据的结构和行为，包括字段、方法和元数据。
- View: 视图，与MVC中C的功能相同，接收请求，进行处理，与M和T进行交互，返回应答. 用于处理请求并生成响应的部分，它们通常是Python函数或类。视图接收请求对象作为参数，并返回响应对象，可以是HTML内容、JSON数据等。
- Template: 模版，和V功能相同，产生HTML页面. 用于生成动态内容的文件，通常包含HTML和模板语言。模板可以包含变量、标签和过滤器，用于动态生成页面内容。
- 中间件Middleware：介于请求和视图之间的一种过滤器。中间件可以对请求进行预处理或后处理，例如身份验证、日志记录等。
流程：
	1. 用户点击注册按钮，将要注册的内容发送给网站的服务器
	2. View 视图，接收到用户发来的注册数据，View 告诉 Model 将用户的注册信息保存进数据库
	3. Model 层将用户的注册信息保存到数据库中
	4. 数据库将保存的结果返回给 Model
	5. Model 将保存的结果给 View 视图
	6. View 视图告诉 Template 模板去产生一个 Html 页面
	7. Template 生成 html 内容返回给 View 视图
	8. View 将 html 页面内容返回给浏览器
	9. 浏览器拿到 view 返回的 html 页面内容进行解析，展示
```
Django中models利用ORM对MySQL进行查表的语句
```python
字段查询
all() 返回模型类对应表格中所有数据
get() 返回满足条件的一条数据，如果查到多条会抛出异常MultipleObjectsReturned，查询不到数据抛出DoesNotExist
"""
filter() 参数写查询条件，返回满足条件QuerySet集合数据
条件格式：模型类属姓名（不是字段名）_条件名=值
"""
BookInfo.object.filter(id=1)
contains BookInfo.obkects.filter(btitle__contains='我') # 模糊查询like
BookInfo.objects.filter(btitle_isnull=False) 
BookInfo.objects.filter(id__in=[1，3，5]) # 范围查询
BookInfo.objects.filter(id__gte=3) # 比较查询gt(>), lt(<), gte(>=), lte
# 日期
BookInfo.objects.filter(bpub_date__year = 1980)
BookInfo.objects.filter(bpub_date__gt = date(1980，1，1))
BookInfo.objects.exclude(id=3) # 返回不满足条件的
"""
F对象：用于类属性之间的比较条件
"""
from django.db.models import F
where bread > bcomment BookInfo.objects.filter(bread__gt =F(‘bcomment’))
BookInfo.objects.filter(bread__gt=F(‘bcomment’)*2)
"""
Q对象：用于查询时的逻辑条件，可以对Q对象进行&|~操作
"""
from django.db.models import Q
BookInfo.objects.filter(id__gt=3， bread__gt=30)
BooInfo.objects.filter(Q(id__gt=3) & Q(bread__gt=3))
例：BookInfo.objects.filter(Q(id__gt=3) | Q(bread__gt=30))
例：BookInfo.objects.filter(~Q(id=3))

'''order_by 返回QuerySet'''
BookInfo.objects.all().order_by('id')
BookInfo.objects.all().order_by('-id')
BookInfo.objects.filter(id__gt=3).order_by('-bread')
'''聚合函数'''
from django.db.models import Sum，Count，Max，Min，Avg
BookInfo.objects.aggregate(Count('id'))
BookInfo.objects.aggregate(Sum(‘bread’))
# 统计所有图书的数目
BookInfo.objects.all().count()
BookInfo.objects.filter(id__gt = 3).count()

模型关系：
一对多 models.ForeignKey() 定义在多的类中。
多对多 models.ManyToManyField() 定义在哪个类中都可以。
一对一 models.OneToOneField() 定义在哪个类中都可以。
```

Django 项目的优化？(这个答案 web 通用)
```
(1) 优化数据库查询: 一次提供所有数据, 仅提供相关的数据
(2) 代码优化: 简化代码, 更新或替代第三方软件包, 重构代码
```
谈一下你对 uWSGI 和 nginx 的理解？
```
uWSGI: 
	- 是一个 Web 服务器，它实现了 WSGI 协议、uwsgi、http 等协议. 
	- Nginx 中 HttpUwsgiModule 的作用是与 uWSGI 服务器进行交换。
	- WSGI 是一种 Web 服务器网关接口。
	- 是一个 Web 服务器（如 nginx，uWSGI 等服务器）与 web 应用（如用 Flask 框架写的程序）通信的一种规范。

nginx: 是一个开源的高性能的 HTTP 服务器和反向代理
	- 作为 web 服务器，它处理静态文件和索引文件效果非常高；
	- 它的设计非常注重效率，最大支持 5 万个并发连接，但只占用很少的内存空间；
	- 稳定性高，配置简洁；
	- 强大的反向代理和负载均衡功能，平衡集群中各个服务器的负载压力应用。
```
WSGI / uwsgi / uWSGI 这三个概念的区分。
```
(1) WSGI 是一种通信协议。
(2) uwsgi 是一种线路协议而不是通信协议，在此常用于在 uWSGI 服务器与其他网络服务器的数据通信。
(3) uWSGI 是实现了 uwsgi 和 WSGI 两种协议的 Web 服务器。
```
说说 nginx 和 uWISG 服务器之间如何配合工作的？
```
1. 首先浏览器发起 http 请求到 nginx 服务器，Nginx 根据接收到请求包，进行 url 分析，判断访问的资源类型。
2. 如果是静态资源，直接读取静态资源返回给浏览器。
3. 如果请求的是动态资源就转交给 uwsgi 服务器，uwsgi 服务器根据自身的 uwsgi 和 WSGI 协议，找到对应的 Django 框架，Django 框架下的应用进行逻辑处理后，将返回值发送到 uwsgi 服务器，然后 uwsgi 服务器再返回给 nginx，最后 nginx 将返回值返回给浏览器进行渲染显示给用户。
```
ngnix 的正向代理与反向代理?
```
web 开发中，部署方式大致类似。简单来说，使用 Nginx 主要是为了实现分流、转发、负载均衡，以及分担服务器的压力。Nginx 部署简单，内存消耗少，成本低。Nginx 既可以做正向代理，也可以做反向代理。
正向代理：请求经过代理服务器从局域网发出，然后到达互联网上的服务器。服务端并不知道真正的客户端是谁。
反向代理：请求从互联网发出，先进入代理服务器，再转发给局域网内的服务器。客户端并不知道真正的服务端是谁。
区别：正向代理的对象是客户端。反向代理的对象是服务端。
```

django 开发中数据库做过什么优化?
```
(1) 设计表时，尽量少使用外键，因为外键约束会影响插入和删除性能
(2) 使用缓存，减少对数据库的访问
(3) 在 orm 框架下设置表时，能用 varchar 确定字段长度时，就别用 text
(4) 可以给搜索频率高的字段属性，在定义时创建索引
(5) Django orm 框架下的 Querysets 本来就有缓存的
(6) 如果一个页面需要多次连接数据库，最好一次性取出所有需要的数据，减少对数据库的查询次数
(7) 若页面只需要数据库里某一个两个字段时，可以用 QuerySet.values()
(8) 在模板标签里使用 with 标签可以缓存 Qset 的查询结果
```

验证码过期时间怎么设置？
```
将验证码保存到数据库或 session，设置过期时间为 1 分钟，然后页面设置一个倒计时(一般是前端js 实现 这个计时)的展示，一分钟过后再次点击获取新的信息。
```

Python 中 Django、Flask、Tornado 三大框架各自的应用场景？
```
Django和Flask将在网络浏览器中键入的URL或地址映射为Python中的函数。

Django
高级的Python Web框架，拥有完整的功能和强大的开发工具。它采用MTV（Model-Template-View）的架构模式，提供了诸如ORM、表单处理、用户认证等功能，适合快速开发大型Web应用。
主要是用来搞快速开发的，他的亮点就是快速开发，节约成本，正常的并发量不过 10000，如果要实现高并发的话，就要对 Django 进行二次开发，比如把整个笨重的框架给拆掉，自己写 socket 实现 http 的通信，底层用纯 c/c++写提升效率，ORM 框架给干掉，自己编写封装与数据库交互的框架，因为啥呢，ORM 虽然面向对象来操作数据库，但是它的效率很低，使用外键来联系表与表之间的查询

Flask:
一个轻量级的Python Web框架，使用简单灵活，适合小型项目和快速原型开发。Flask提供了基本的功能，如路由、模板、请求和响应等，同时支持扩展以满足不同需求。
一种微框架，主要用于具有更简单要求的小型应用系统。必须使用外部库
轻量级，主要是用来写接口的一个框架，实现前后端分离，提升开发效率，Flask 本身相当于一个内核，其他几乎所有的功能都要用到扩展（邮件扩展 Flask-Mail，用户认证 Flask-Login），都需要用第三方的扩展来实现。比如可以用 Flask-extension 加入 ORM、窗体验证工具，文件上传、身份验证等。Flask 没有默认使用的数据库，你可以选择 MySQL，也可以用 NoSQL。

其 WSGI 工具箱采用 Werkzeug（路由模块），模板引擎则使用 Jinja2。这两个也是 Flask 框架的核心。Python 最出名的框架要数 Django，此外还有 Flask、Tornado 等框架。虽然 Flask 不是最出名的框架，但是 Flask 应该算是最灵活的框架之一，这也是 Flask 受到广大开发者喜爱的原因。

Tornado
一个高性能的Python Web框架和异步网络库，适用于需要处理大量并发连接的应用。它可以用作Web服务器，也可以用作Web应用程序框架。
Tornado 是一种 Web 服务器软件的开源版本。
Tornado 和现在的主流 Web 服务器框架（包括大多数 Python 的框架）有着明显的区别：它是非阻塞式服务器，而且速度相当快。得利于其非阻塞的方式和对 epoll 的运用，Tornado 每秒可以处理数以千计的连接，因此 Tornado 是实时 Web 服务的一个 理想框架。

Pyramid：金字塔是为较大的程序而构建的。提供了灵活性，并允许开发人员为它们的项目使用正确的工具，可配置。一个通用的Python Web应用程序框架，它可以适应不同规模的应用。Pyramid提供了灵活的路由配置、模板渲染、安全认证等功能，可以根据需要进行扩展

Bottle: 是一个简单、轻量级的Python Web框架，适合用于快速开发小型Web应用。它支持路由、模板、请求和响应处理等基本功能，同时可以与其他工具和库集成。
```

Django的生命周期
```
Django 是一个开源的 Web 应用框架，其生命周期包括以下几个阶段：

1. 项目初始化：使用 `django-admin startproject <project_name>` 命令创建一个 Django 项目。使用 `python manage.py startapp <app_name>` 命令创建一个 Django 应用程序。
2. 配置：
    - 设置数据库配置：在 `settings.py` 文件中配置数据库信息。
    - 配置应用程序：在 `INSTALLED_APPS` 中注册应用程序。
    - 配置 URL 映射：在 `urls.py` 文件中配置 URL 映射关系。
3. 开发：
    - 编写视图函数：在应用程序的 `views.py` 文件中编写视图函数，处理请求并返回响应。
    - 定义模型：在应用程序的 `models.py` 文件中定义模型类，表示数据库中的数据结构。
    - 编写模板：在应用程序的 `templates` 目录下编写 HTML 模板文件，用于页面展示。
4. 测试：
    - 编写测试用例：在应用程序的 `tests.py` 文件中编写测试用例，测试应用程序的功能是否正常。
    - 运行测试：使用 `python manage.py test` 命令运行测试用例，验证应用程序的功能。
5. 部署：
    - 配置服务器环境：将 Django 项目部署到生产环境，配置服务器环境，包括 Web 服务器、数据库服务器等。
    - 收集静态文件：使用 `python manage.py collectstatic` 命令收集静态文件，用于生产环境。
6. 维护：
    - 监控和调优：监控 Django 项目的性能和运行状态，根据需要进行调优。
    - 定期更新：定期更新 Django 版本和相关库，确保项目安全性和稳定性。
7. 结束：在项目完成或终止时，进行总结和文档整理，归档项目文件并关闭服务器。
```
Django中继承样式
```
1. 抽象基类：当您只希望父类的类保留您不想为每个子模型键入的信息时，使用此样式。
2. 多表继承：如果要对现有模型进行子类化并且需要每个模型都有自己的数据库表，则使用此样式。
3. 代理模型：如果只想修改模型的Python级别行为，而不更改模型的字段，则可以使用此模型
```
django 如何提升性能？
```
后端
提升性能指标主要有两个一个是并发数，另一个是响应时间网站性能的优化一般包括 web 前端性能优化，应用服务器性能优化，存储服务器优化。

前端
(1) 减少 http 请求，减少数据库的访问量，比如使用雪碧图。
(2)使用浏览器缓存，将一些常用的 css，js，logo 图标，这些静态资源缓存到本地浏览器，通过设置 http 头中的 cache-control 和 expires 的属性，可设定浏览器缓存，缓存时间可以自定义。
(3) 对 html，css，javascript 文件进行压缩，减少网络的通信量。

总体：
(1)合理的使用缓存技术，对一些常用到的动态数据，比如首页做一个缓存，或者某些常用的数据做个缓存，设置一定得过期时间，这样减少了对数据库的压力，提升网站性能。
(2) 使用 celery 消息队列，将耗时的操作扔到队列里，让 worker 去监听队列里的任务，实现异步操作，比如发邮件，发短信。
(3) 就是代码上的一些优化，补充：nginx 部署项目也是项目优化，可以配置合适的配置参数，提升效率，增加并发量。
(4) 如果太多考虑安全因素，服务器磁盘用固态硬盘读写，远远大于机械硬盘，这个技术现在没有普及，主要是固态硬盘技术上还不是完全成熟， 相信以后会大量普及。
(5) 另外还可以搭建服务器集群，将并发访问请求，分散到多台服务器上处理。
(6) 最后就是运维工作人员的一些性能优化技术了。
```
**Django的优点是什么？**
```
- 提供了完善的开发文档和丰富的开发工具。
- 自带强大的ORM（对象关系映射）系统，支持多种数据库后端。
- 提供了自动生成管理界面的功能，方便管理数据。
- 拥有丰富的第三方插件和扩展，可扩展性强。
- 自带强大的表单处理功能，简化了表单验证和数据处理的流程。
```
如何在Django中创建一个新的应用程序？
```
在Django中，可以使用`python manage.py startapp <app_name>`命令来创建一个新的应用程序。该命令将在项目目录下创建一个新的应用程序文件夹，包含默认的应用程序结构。
```

启动 Django 服务的方法？
```
runserver 方法是调试 Django 时经常用到的运行方式，它使用 Django 自带的 WSGI Server 运行，主要在测试和开发中使用，并且 runserver 开启的方式也是单进程 。
```
怎样测试 django 框架中的代码？
```
在单元测试方面，Django 继承 python 的 unittest.TestCase 实现了自己的　django.test.TestCase，编写测试用例通常从这里开始。测试代码通常位于 app 的 tests.py 文件中(也可以在 models.py 中编写，一般不建议）。在 Django 生成的 depotapp 中，已经包含了这个文件，并且其中包含了一个测试
python manage.py test：执行所有的测试用例
python manage.py test app_name， 执行该 app 的所有测试用例
python manage.py test app_name.case_name: 执行指定的测试用例

一些测试工具：unittest 或者 pytest
```

Django 中哪里用到了线程?哪里用到了协程?哪里用到了进程？
```
(1) Django 中耗时的任务用一个进程或者线程来执行，比如发邮件，使用 celery。
(2) 部署 django 项目的时候，配置文件中设置了进程和协程的相关配置。
```

django 关闭浏览器，怎样清除 cookies 和 session？
```python
# 设置 Cookie
def cookie_set(request):
	response = HttpResponse("<h1>设置 Cookie，请查看响应报文头</h1>")
	response.set_cookie('h1', 'hello django')
	return response
# 读取Cookie
def cookie_get(request):
	response = HttpResponse("读取 Cookie，数据如下：<br>")
	if request.COOKIES.has_key('h1'):
		response.write('<h1>' + request.COOKIES['h1'] + '</h1>')
	return response

# 以键值对的格式写会话
request.session['键']=值
# 根据键读取值
request.session.get('键',默认值)
# 清除所有会话，在存储中删除值部分
request.session.clear()
# 清除会话数据，在存储中删除会话的整条数据
request.session.flush()
# 删除会话中的指定键及值，在存储中只删除某个键及对应的值
del request.session['键']

'''
设置会话的超时时间，如果没有指定过期时间则两个星期后过期。
如果 value 是一个整数，会话将在 value 秒没有活动后过期。
如果 value 为 0，那么用户会话的 Cookie 将在用户的浏览器关闭时过期。
如果 value 为 None，那么会话永不过期。
'''
request.session.set_expiry(value)

'''
Session 依赖于 Cookie，如果浏览器不能保存 cookie 那么 session 就失效了。因为它需要浏览器的 cookie 值去 session 里做对比。session 就是用来在服务器端保存用户的会话状态。

cookie 可以有过期时间，这样浏览器就知道什么时候可以删除 cookie 了。 如果 cookie 没有设置过期时间，当用户关闭浏览器的时候，cookie 就自动过期了。
可以改变 SESSION_EXPIRE_AT_BROWSER_CLOSE 的设置来控制 session 框架的这一行为。缺省情况下，SESSION_EXPIRE_AT_BROWSER_CLOSE 设置为 False ，这样，会话 cookie 可以在用户浏览器中保持有效达 SESSION_COOKIE_AGE 秒（缺省设置是两周，即 1，209，600 秒）如果你不想用户每次打开浏览器都必须重新登陆的话，用这个参数来帮你。如果SESSION_EXPIRE_AT_BROWSER_CLOSE 设置为 True，当浏览器关闭时，Django 会使 cookie 失效。
SESSION_COOKIE_AGE：设置 cookie 在浏览器中存活的时间。
'''
```


Django中间件的使用
```python
'''
Django 在中间件中预置了六个方法，这六个方法的区别在于不同的阶段执行，对输入或输出进行干预.
'''
def __init__(): 初始化：无需任何参数，服务器响应第一个请求的时候调用一次，用于确定是否启用当前中间件。
def process_request(request): 处理请求前：在每个请求上调用，返回 None 或 HttpResponse 对象。
def process_view(request, view_func, view_args, view_kwargs): 处理视图前：在每个请求上调用，返回 None 或 HttpResponse 对象。
def process_template_response(request, response): 处理模板响应前：在每个请求上调用，返回实现了 render 方法的响应对象。
def process_response(request, response): 处理响应后：所有响应返回浏览器之前被调用，在每个请求上调用，返回 HttpResponse 对象。
def process_exception(request,exception): 异常处理：当视图抛出异常时调用，在每个请求上调用，返回一个 HttpResponse 对象。

# 中间件不用继承自任何类（可以继承 object）
class CommonMiddleware(object):
    def process_request(self， request):
        return None

def process_response(self， request， response):
    return response

```

简述 Django 下的（内建的）缓存机制?
```
一个动态网站的基本权衡点就是，它是动态的。每次用户请求页面，服务器会重新计算。从开销处理的角度来看，这比你读取一个现成的标准文件的代价要昂贵的多。这就是需要缓存的地方。

Django 自带了一个健壮的缓存系统来保存动态页面这样避免对于每次请求都重新计算。方便起见，Django 提供了不同级别的缓存粒度：可以缓存特定视图的输出、可以仅仅缓存那些很难生产出来的部分、或者可以缓存整个网站 Django 也能很好的配合那些“下游”缓存， 比如 Squid 和基于浏览器的缓存。这里有一些缓存不必要直接去控制但是可以提供线索， (via HTTPheaders)关于网站哪些部分需要缓存和如何缓存。

设置缓存：缓存系统需要一些设置才能使用。 也就是说，你必须告诉他你要把数据缓存在哪里- 是数据库中，文件系统或者直接在内存中。这个决定很重要，因为它会影响你的缓存性能，是的，一些缓存类型要比其他的缓存类型更快速。
你的缓存配置是通过 setting 文件的 CACHES 配置来实现的。这里有 CACHES 所有可配置的变量值。
```

Django HTTP 请求的处理流程？
```
和其他 Web 框架的 HTTP 处理的流程大致相同，Django 处理一个 Request 的过程是首先通过中间件，然后再通过默认的 URL 方式进行的。我们可以在 Middleware 这个地方把所有 Request 拦截住，用我们自己的方式完成处理以后直接返回 Response。
(1) 加载配置
Django 的配置都在 “Project/settings.py” 中定义，可以是 Django 的配置，也可以是自定义的配置，并且都通过 django.conf.settings 访问，非常方便。
(2) 启动
最核心动作的是通过 django.core.management.commands.runfcgi 的 Command 来启动，它运行 django.core.servers.fastcgi 中的 runfastcgi，runfastcgi 使用了 flup 的 WSGIServer 来启动 fastcgi 。而 WSGIServer 中携带了 django.core.handlers.wsgi 的 WSGIHandler 类的一个实例，通过 WSGIHandler 来处理由 Web 服务器（比如 Apache，Lighttpd 等）传过来的请求，此时才是真正进入 Django 的世界。
(3) 处理 Request
当有 HTTP 请求来时，WSGIHandler 就开始工作了，它从 BaseHandler 继承而来。WSGIHandler 为每个请求创建一个 WSGIRequest 实例，而 WSGIRequest 是从 http.HttpRequest 继承而来。接下来就开始创建 Response 了。
(4) 创建 Response
BaseHandler 的 get_response 方法就是根据 request 创建 response，而具体生成 response 的动作就是执行 urls.py 中对应的 view 函数了，这也是 Django 可以处理“友好 URL ”的关键步骤，每个这样的函数都要返回一个 Response 实例。此时一般的做法是通过 loader 加载 template 并生成页面内容，其中重要的就是通过 ORM 技术从数据库中取出数据，并渲染到 Template 中，从而生成具体的页面了。
(5) 处理 Response
Django 返回 Response 给 flup，flup 就取出 Response 的内容返回给 Web 服务器，由后者返回给浏览器。

总之，Django 在 fastcgi 中主要做了两件事：处理 Request 和创建 Response，而它们对应的核心就是“ urls 分析”、“模板技术”和“ ORM 技术”。
```
Django 里 QuerySet 的 get 和 filter 方法的区别？
```
(1) 输入参数
get: 只能是 model 中定义的那些字段，只支持严格匹配。
filter :可以是字段，也可以是扩展的 where 查询关键字，如 in，like 等。
(2) 返回值
get : 一个定义的 model 对象。
filter : 一个新的 QuerySet 对象，然后可以对 QuerySet 在进行查询返回新的 QuerySet 对象，支持链式操作，QuerySet 一个集合对象，可使用迭代或者遍历，切片等，但是不等于 list 类型(使用一定要注意)。
(3) 异常
get : 只有一条记录返回的时候才正常，也就说明 get 的查询字段必须是主键或者唯一约束的字段。当返回多条记录或者是没有找到记录的时候都会抛出异常 
filter :有没有匹配的记录都可以
```

跨域请求问题 django 怎么解决的？
```
启用中间件
post 请求
验证码
表单中添加 csrf_token 标签
```

Django 对数据查询结果排序怎么做，降序怎么做，查询大于某个字段怎么做？
```
排序使用 order_by()
降序需要在排序字段名前加-
查询字段大于某个值：使用 filter(字段名_gt=值)
```

Django 重定向你是如何实现的？用的什么状态码？
```
使用 HttpResponseRedirect
redirect 和 reverse
状态码：302,301
```

生成迁移文件和执行迁移文件的命令是什么？
```python
python manage.py makemigrations
python manage.py migrate
```

查询集返回列表的过滤器有哪些？
```
all() ：返回所有的数据
filter()：返回满足条件的数据
exclude()：返回满足条件之外的数据，相当于 sql 语句中 where 部分的 not 关键字
order_by()：排序
```

Django 本身提供了 runserver，为什么不能用来部署？
```
runserver 方法是调试 Django 时经常用到的运行方式，它使用 Django 自带的 WSGI Server 运行，主要在测试和开发中使用，并且 runserver 开启的方式也是单进程 。

uWSGI 是一个 Web 服务器，它实现了 WSGI 协议、uwsgi、http 等协议。注意 uwsgi 是一种通信协议，而 uWSGI 是实现 uwsgi 协议和 WSGI 协议的 Web 服务器。uWSGI 具有超快的性能、低内存占用和多 app 管理等优点，并且搭配着 Nginx 就是一个生产环境了，能够将用户访问请求与应用 app 隔离开，实现真正的部署。相比来讲，支持的并发量更高，方便管理多进程，发挥多核的优势，提升性能。
```

HttpRequest 和 HttpResponse 是什么？干嘛用的？
```
HttpRequest 是 django 接受用户发送多来的请求报文后，将报文封装到 HttpRequest 对象中去。
HttpResponse 返回的是一个应答的数据报文。render 内部已经封装好了 HttpResponse 类。
HttpResponse 常见属性：
content： 表示返回的内容
charset: 表示 response 采用的编码字符集，默认是 utf-8
status_code:返回的 HTTP 响应状态码 3XX 是对请求继续进一步处理，常见的是重定向。


视图的第一个参数必须是 HttpRequest 对象，两点原因：表面上说，他是处理 web 请求的，所以必须是请求对象，根本上说，他是基于请求的一种 web 框架，所以，必须是请求对象。
因为 view 处理的是一个 request 对象，请求的所有属性我们都可以根据对象属性的查看方法来获取具体的信息：格式：request.属性
request.path 请求页面的路径，不包含域名
request.get_full_path 获取带参数的路径
request.method 页面的请求方式
request.GET GET 请求方式的数据
request.POST POST 请求方式的数据
request.COOKIES 获取 cookie
request.session 获取 session
request.FILES 上传图片（请求页面有 enctype="multipart/form-data"属性时 FILES 才有数据。？a=10 的键和值时怎么产生的，键是开发人员在编写代码时确定下来的，值时根据数据生成或者用户填写的，总之是不确定的。

request.GET.get()取值时如果一键多值情况，get 是覆盖的方式获取的。getlist（）可以获取多值。在一个有键无值的情况下，该键名 c 的值返回空。有键无值：c: getlist 返回的是列表，空列表在无键无值也没有默认值的情况下，返回的是 None 无键无值：e:None


常见方法：
init:创建 httpResponse 对象完成返回内容的初始化
set_cookie：设置 Cookie 信息：格式：set_cookies('key','value',max_age=None,expires=None)
max_age 是一个整数，表示指定秒数后过期，expires 指定过期时间，默认两个星期后过期。
write 向响应体中写数据
```

403 错误
```
表示资源不可用，服务器理解客户的请求，但是拒绝处理它，通常由于服务器上文件和目录的权限设置导致的 web 访问错误。
如何解决：
(1) 把中间件注释。
(2) 在表单内部添加{% scrf_token %}
```

应答对象：
```
方式一：render(request,“index.html”) 返回一个模板
render(request,“index.html”, context) 返回一个携带动态数据的页面
方式二：render_to_response(“index.html”) 返回一个模板页面
方式三：redirect("/") 重定向
方式四：HttpResponseRdeirect("/") 实现页面跳转功能
方式五：HttpResponse（“itcast1.0”)在返回到额页面中添加字符串内容
方式六：HttpResponseJson() 返回的页面中添加字符串内容。

JsonResponse 创建对象时候接收字典作为参数，返回的对象是一个 json 对象。
能接收 Json 格式数据的场景，都需要使用 view 的 JsonResponse 对象返回一个 json 格式数据
ajax 的使用场景，页面局部刷新功能。ajax 接收 Json 格式的数据。
在返回的应答报文中，可以看到 JsonResponse 应答的 content-Type 内容是 application/json
ajax 实现网页局部刷新功能：ajax 的 get()方法获取请求数据 ajax 的 each()方法遍历输出这些数据
```

Django 日志管理
```
# 配置好之后
import logging
logger=logging.getLogger(__name__) # 为 loggers 中定义的名称
logger.info("some info ...)
# 可用函数有：logger.debug() logger.info() logger.warning() logger.error()
```


Django 文件管理
```
对于 jdango 老说，项目中的 css，js,图片都属于静态文件，我们一般会将静态文件放到一个单独的目录中，以方便管理，在 html 页面调用时，也需要指定静态文件的路径。静态文件可以放在项目根目录下，也可以放在应用的目录下，由于这些静态文件在项目中是通用的，所以推荐放在项目的根目录下。

在生产中，只要和静态文件相关的，所有访问，基本上没有 django 什么事，一般都是由 nignx 软件代劳了，为什么？因为 nginx 就是干这个的。
```


