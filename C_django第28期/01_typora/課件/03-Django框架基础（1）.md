# 1 初识Django

* Django 是使用 Python 语言开发的一款免费而且开源的 Web 应用框架。

* 由于 Python 语言的跨平台性，所以 Django 同样支持 Windows、Linux 和 Mac 系统。

## 1.1 框架的特点

相对于 Python 的其他 Web 框架，Django 的功能是最完整的，Django 定义了服务发布、路由映射、模板编程、数据处理的一整套功能。

Django的主要特点如下：

+ 完善的文档：经过多年的发展和完善，Django 官方提供了完善的在线文档，为开发者解决问题提供支持。
+ 集成 ORM 组件：Django 的 Model 层自带数据库 ORM 组件，为操作不同类型的数据库提供了统一的方式。

+ URL 映射技术：Django 使用正则表达式管理URL映射，因此给开发者带来了极高的灵活性。
+ 后台管理系统：开发者只需通过简单的几行配置和代码就可以实现完整的后台数据管理Web控制台。
+ 错误信息提示：在开发调试过程中如果出现异常，Django 可以提供完整的错误信息帮助开发者定位问题。

## 1.2 设计模式

- 本节我们介绍 Django 的设计模式，也就是 MTV。
- 在 Web 开发领域还有另外一个非常著名的设计模式——MVC，它和 MTV 又有什么区别呢？

- MVC设计模式与MTV设计模式是本节的重点


### 1.2.1 MVC设计模式

我们先对 MVC 设计模式进行介绍，它是 Web 设计模式的经典之作，MTV 模式也是在它的基础上衍生而来。

MVC 是 Model-View-Controller 的缩写，其中每个单词都有其不同的含义：

+ Modle 代表数据存储层，是对数据表的定义和数据的增删改查；
+ View 代表视图层，是系统前端显示部分，它负责显示什么和如何进行显示；
+ Controller 代表控制层，负责根据从 View 层输入的指令来检索 Model 层的数据，并在该层编写代码产生结果并输出。

![](imgs\企业微信截图_20230406145358.png)

MVC 设计模式的请求与响应过程描述如下：

1. 用户通过浏览器向服务器发起 request 请求，Controller 接受请求后，同时向 Model 和 View 发送指令
2. Model 根据指令与数据库交互并选择相应业务数据，然后将数据发送给 Controller 
3. View 接收到 Controller 的指令后，加载用户请求的页面，并将此页面发送给 Controller 
4. Controller 接收到 Model 和 View 的数据后，将它们组织成响应格式发送给浏览器，浏览器通过解析后把页面展示出来

MVC 的 3 层之间紧密相连，但又相互独立，每一层的修改都不会影响其它层，每一层都提供了各自独立的接口供其它层调用，MVC 的设计模式降低了代码之间的耦合性（即关联性），增加了模块的可重用性，这就是 MVC 的设计模式。

### 1.2.2 MTV设计模式

那么 Django 的 MTV 又是怎么回事呢？下面讲解 Django 的设计模式。

Django 借鉴了经典的 MVC 模式，它也将交互的过程分为了 3 个层次，也就是 MTV 设计模式：

+ Model：数据存储层，处理所有数据相关的业务，和数据库进行交互，并提供数据的增删改查
+ Template：模板层（也叫表现层）具体来处理页面的显示
+ View：业务逻辑层，处理具体的业务逻辑，它的作用是连通Model 层和 Template

MTV 设计模式示意图如下：

![](imgs\企业微信截图_20230406151526.png)

我们按照 MVC 的设计模式对 MTV 进行分析，MTV 设计模式中，用 View 层取代了 Controller 层的位置，用 Template 层取代了原来 View 层的位置。

初次接触 Django 的设计模式的人，可能会对 Template 层产生疑问，其实 Template 英文的含义就是“模板”的意思，你可以简单理解成，它是一个 HTML 页面 ，HTML 页面的渲染在视图层完成。

MTV 设计模式的请求与响应过程描述如下：

1. 用户通过浏览器对服务器发起 request 请求，服务器接收请求后，通过 View 的业务逻辑层进行分析，同时向 Model 层和 Template 层发送指令；
2. Mole 层与数据库进行交互，将数据返回给 View 层；
3. Template 层接收到指令后，调用相应的模板，并返回给 View 层；
4. View 层接收到模板与数据后，首先对模板进行渲染（即将相应的数据赋值给模板），然后组织成响应格式返回给浏览器，浏览器进行解析后并最终呈现给用户。

通过以上两种设计模式的比较， 我们可以得出 MTV 是 MVC 的一种细化，将原来 MVC 中的 V 层拿出来进行分离，视图的显示与如何显示交给 Template 层，而 View 层更专注于实现业务逻辑。其实在 Django 是有 Controller 层的，只不过它由框架本身来实现，所以我们不用关心它。Django 更关注于M、T 和 V。

# 2 Django入门

## 2.1 安装

~~~python
pip install django==4.1.3 -i 源镜像
~~~

## 2.2 创建项目

~~~
1、打开cmd，cd到指定的文件夹

2、输入：django-admin startproject 项目名
~~~

## 2.3 项目文件介绍

~~~
my_project 			 【项目名】
	manage.py		 【不动，项目管理，项目启动，创建app(应用)，数据管理】
	my_project
		settings.py   【配置文件，连接数据库】
		urls.py       【URL和python函数的对应关系】
		asgi.py		 【接受网络请求】【异步】
		wsgi.py       【接受网络请求】【同步】
~~~

## 2.4 启动项目

~~~
cd 到项目文件夹
D:\python_class\12_class\web后端\4、Django基础\my_project> python manage.py runserver [ip地址:端口]
如果ip地址为0.0.0.0，进入项目的时候，可以通过本机ipv4访问
~~~

![](.\imgs\启动项目.png)

# 3 APP(应用)

* 在大型项目当中，为项目中的一些子应用创建app，方便管理
  * -- app 用户管理
  * -- app 订单业务
  * -- app 后台管理
  * -- app 业务管理

## 3.1 创建方式

~~~
1、cd到项目目录
D:\python_class\12_class\web后端\4、Django基础\my_project> python manage.py startapp app名
~~~

## 3.3 文件介绍

~~~
project_one
	migrations   【数据库变更记录】
	admin.py	【不动，django提供的默认的admin后台管理】
	apps.py      【app启动类】  
	models.py    【数据库的操作】【封装ORM】
	tests.py     【单元测试 不动】
	views.py     【视图函数】
~~~

* #### 注册app

![](.\imgs\注册app.png)

## 3.4 编写url和视图函数的对应关系

![](D:\python_class_note\全栈开发-2.0\4、Django框架基础\imgs\url与视图函数的对应.png)

* ###### 在app当中创建templates文件夹

  存储html模板

  ## 3.5 页面跳转


* #### 创建static文件夹

  存放css，js，img

  ![](.\imgs\static.png)









​	

​	



