## 项目目录介绍
```
mysite
    - mysite    # 对整个程序进行配置
    - init      #一个空文件，它告诉Python这个目录应该被看做一个Python包
    - settings    # 项目配置文件
    - url      # URL对应关系（路由）
    - wsgi     # 遵循WSIG规范，uwsgi + nginx
- manage.py     # 一个命令行工具，可以使你用多种方式对Django项目进行交互mysite
    - mysite    # 对整个程序进行配置
    - init      #一个空文件，它告诉Python这个目录应该被看做一个Python包
    - settings    # 项目配置文件
    - url      # URL对应关系（路由）
    - wsgi     # 遵循WSIG规范，uwsgi + nginx
- manage.py     # 一个命令行工具，可以使你用多种方式对Django项目进行交互
```

## 应用目录及文件介绍:
```
blog                 #应用目录
│  admin.py        #对应应用后台管理配置文件。
│  apps.py         #对应应用的配置文件。
│  models.py       #数据模块，数据库设计就在此文件中设计。后面重点讲解
│  tests.py        #自动化测试模块，可在里面编写测试脚本自动化测试
│  views.py        #视图文件，用来执行响应代码的。你在浏览器所见所得都是它处理的。
│  __init__.py
│
├─migrations        #数据迁移、移植文目录，记录数据库操作记录，内容自动生成。
│  │  __init__.py
```
## Django常用的命令:
```
安装Django： pip install django  指定版本 pip3 install django==2.0
新建项目： django-admin.py startproject mysite
新建APP : python manage.py startapp blog
启动：python manage.py runserver 0.0.0.0:8080
同步或者更改生成 数据库：
   python manage.py makemigrations
   python manage.py migrate
清空数据库： python manage.py flush
创建管理员： python manage.py createsuperuser
修改用户密码： python manage.py changepassword username
Django项目环境终端： python manage.py shell
这个命令和 直接运行 python 进入 shell 的区别是：你可以在这个 shell 里面调用当前项目的 models.py 中的 API，对于操作数据的测试非常方便。

```

##  Python requestment.txt文件的生成和安装

```
生成：pip freeze > requirements.txt
安装：pip install -r requirements.txt
```


## 当使用makemigrations时报错No changes detected
```
在修改了models.py后，有些用户会喜欢用python manage.py makemigrations生成对应的py代码。
但有时执行python manage.py makemigrations命令（也可能人比较皮，把migrations文件夹给删了），会提示"No changes detected." 可能有用的解决方式如下
python manage.py makemigrations --empty yourappname 生成一个空的initial.py
python manage.py makemigrations 生成原先的model对应的migration file
python manage.py migrate
```

## 自定义表名(models.py)

```python
class Message(models.Model):
	name = models.CharField(max_length=20, verbose_name='姓名')
	email = models.EmailField(verbose_name='邮箱')
	address = models.CharField(max_length=200, verbose_name='联系地址')
	message = models.TextField(verbose_name='留言信息')

	class Meta:
		verbose_name = '留言信息'
		verbose_name_plural = verbose_name
		#自定义表名
		db_table = "message"

```

## 常见报错

### `ImproperlyConfigured: mysqlclient 1.3.13 or newer is required`

python3 manage.py migrate` 进行数据迁移时报如下错误：

```
......

  File "/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/importlib/__init__.py", line 127, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages/django/db/backends/mysql/base.py", line 36, in <module>
    raise ImproperlyConfigured('mysqlclient 1.3.13 or newer is required; you have %s.' % Database.__version__)
django.core.exceptions.ImproperlyConfigured: mysqlclient 1.3.13 or newer is required; you have 0.9.3.

```

虽然本地已安装了 PyMySQL 驱动，但 Django 连接 MySQL 时仍默认使用 MySQLdb 驱动，但 MySQLdb 并不支持 Python3，所以需要手动在项目中进行配置。

在项目根目录下的 `__init__.py` 文件中添加如下代码即可：

```
import pymysql
pymysql.install_as_MySQLdb()
```

### raise ImproperlyConfigured('mysqlclient 1.3.13 or newer is required; you     have %s.' % Database.__version__)

再次执行命令时，还是会报错，没关系，仔细看下报错的倒数第三行，已经告诉你是在 [base.py](http://base.py/) 第 36 行报的错，根据你的提示路径打开 [base.py](http://base.py/)，把 35、36 行前面加 # 注释掉就好了，就像下面这样

```
34 version = Database.version_info
 35 #if version < (1, 3, 13):
 36 #    raise ImproperlyConfigured('mysqlclient 1.3.13 or newer is required; you     have %s.' % Database.__version__)
 
```

### AttributeError: 'str' object has no attribute 'decode

```
File "/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages/django/db/backends/mysql/operations.py", line 146, in last_executed_query
    query = query.decode(errors='replace')
AttributeError: 'str' object has no attribute 'decode'


```

提示属性错误:“str”对象没有属性“decode”。

问题的原因是，在 Python3 里：str 通过 encode() 转换成 bytes,bytes 通过 decode() 转换成 str,也就是说：str 只有 encode() 方法，bytes 只有 decode() 方法！
这个估计是 django 的 bug 了。

tips：
str通过encode()方法可以编码为指定的bytes。反过来，当从网络或磁盘上读取了字节流，那么读到的数据就是bytes。
要把bytes变为str，就需要用decode()方法。反之，则使用encode()方法即可！解决方法：

根据提示打开报错的文件 operations.py,找到 146 行，把 decode 改成 encode 即可，类似下面这样

```
140     def last_executed_query(self, cursor, sql, params):
141         # With MySQLdb, cursor objects have an (undocumented) "_executed"
142         # attribute where the exact query sent to the database is saved.
143         # See MySQLdb/cursors.py in the source distribution.
144         query = getattr(cursor, '_executed', None)
145         if query is not None:
146             query = query.encode(errors='replace')	# 这里把 decode 改为 encode
147         return query

```



## Django REST framework 简介

```
Django REST framework 简介
序列化和反序列化可以复用
增：效验请求数据 > 执行反序列化过程 > 保存数据库 > 将保存的对象序列化并返回
删：判断要删除的数据是否存在 > 执行数据库删除
改：判断要修改的数据是否存在 > 效验请求的参数 > 执行反序列化过程 > 保存数据库 > 将保存的对象序列化并返回
查：查询数据库 > 将数据序列化并返回
特点:

提供了定义序列化器Serializer的方法,可以快速根据Django ORM 或者其他库自动序列化/反序列化
提供了丰富的类视图\MIXIN扩展类,简化视图的编写
丰富的定制层级:函数视图\类视图\试图结合到自动生成API,满足各种需要
多种身份认证和权限认证方式的支持
内置了限流系统
直观的API web界面
可扩展性 , 插件丰富

序列化器（Serializer） 
　　1. 自定义型:  继承rest_framework.serializers.Serializer
　　2. 模型类型:  继承rest_framework.serializers.ModelSerializer
```

## django Q 查询 filter 常用查询条件

```
__exact 精确等于      like 'aaa'
 __iexact 精确等于    忽略大小写 ilike 'aaa'
__contains 包含 like '%aaa%'
__icontains 包含        忽略大小写 ilike '%aaa%'，
__gt 大于
__gte 大于等于
__lt 小于
__lte 小于等于
__in 存在于一个list范围内
__startswith 以...开头
__istartswith 以...开头 忽略大小写
__endswith 以...结尾
__iendswith 以...结尾，忽略大小写
__range 在...范围内
__year 日期字段的年份
__month 日期字段的月份
__day 日期字段的日
__isnull=True/False
```

