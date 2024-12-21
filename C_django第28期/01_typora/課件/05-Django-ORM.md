## 1 什么是ORM

ORM中文---对象-关系映射

在MTV,MVC设计模式当中，在model模块当中都包含ORM

pip install mysqlclient==2.2.0

### 1.1 ORM优势

* 只需要面向对象编程，不需要面向数据库编写代码
  * 对数据库的操作都可以转化成类属性，类方法
  * 不用编写SQL语句

* 实现了数据模型与数据库的解耦，还屏蔽了不同数据库操作上的差异
  * 不再关注使用的是MySQL oracle....
  * 通过简单的代码配置，轻松跟换数据库，不需要频繁修改代码

### 1.2ORM 劣势

* 对比直接使用SQL语句，有一些性能上的损失
* 根据代码的执行，将结果转化成对象，在映射过程当中存在性能的损失

### 1.3 ORM与数据库的关系

* 在Django当中，model是数据单一，明确的来源，存储了数据的一些重要的字段和行为

## 2 ORM

### 2.1 作用

~~~
1、创建，修改，删除数据库当中的表 【无法创建数据库】
2、操作表中的数据			   【无需编写SQL代码】
	确保服务正常启动
~~~

### 2.2 连接数据库

~~~python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'dbname',
        'USER': '用户名',
        'PASSWORD': '密码',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}
~~~

### 2.3 表操作

![](.\image\创建表.png)

### 2.4 数据库的迁移

* 确保app已经注册

  python manage.py makemigrations

  python manage.py migrate

* 表字段的增加
  * 手动输入一个默认值
  * 给定默认值 defaule=10
  * 允许为空