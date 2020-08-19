## django虚拟环境安装

```python
Virtualenv的安装很简单，一行命令就能搞定：
pip install virtualenv
Virtualenv安装好之后，就是给自己的项目创建一个虚拟环境。
virtualenv pyweb  #pyweb  为虚拟环境目录名，目录名自定义.
当然你也可以使用下面的命令创建指定Python版本的虚拟环境。
virtualenv --python=/usr/bin/python3.6 pyweb    #指定创建一个版本为python3.6的虚拟环境

启动虚拟环境:

Windows下：
进入pyweb目录下的Scripts目录下。
然后输入：activate 回车，就能启动虚拟环境。
至于退出虚拟环境，使用如下命令即可！
deactivate

Linux下：
我们进入创建的虚拟环境的bin目录下，然后使用如下命令启动
source activate

生产requirements.txt文件：
pip freeze > requirements.txt
#将requirements.txt拷贝到B安装
#该命令可以跳过安装错误的库，继续安装
while read requirement; do sudo pip install $requirement; done < requirements.txt   


```




## django项目目录介绍

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

### error: command 'gcc' failed with exit status 1

```
安装运行库
yum install gcc libffi-devel python-devel openssl-devel -y

若python3是使用yum安装的，则安装python3的devel环境
yum install  python3-devel -y
```





# Django REST framework



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
__exact 精确等于      like 'aaa'
 __iexact 精确等于    忽略大小写 ilike 'aaa'
__contains 包含 like '%aaa%'
__icontains 包含        忽略大小写 ilike '%aaa%'，
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

## django-view

```
django:
view:Apiview

drf(继承关系)
GenericAPIViewSet(viewset)			--drf
	GenericAPIView(APIView)			--drf
		APIView(View)				--drf
			View(object)			--django
						
mixins
	CreateModelMixin
	ListModelMixin
	RetrieveModelMixin
	UpdateModelMixin
	DestroyModelMixin
	
Mixin和View的职能区分为：Mixin提供数据，View提供模板和渲染
```

## drf 的过滤

```

```

## django 跨域问题

```
django 跨域问题：（django-cors-headers）

什么是跨域？
跨域是指一个域下的文档或脚本试图去请求另一个域下的资源，这里跨域是广义的。

广义的跨域：

1.) 资源跳转： A链接、重定向、表单提交
2.) 资源嵌入： <link>、<script>、<img>、<frame>等dom标签，还有样式中background:url()、@font-face()等文件外链
3.) 脚本请求： js发起的ajax请求、dom和js对象的跨域操作等

我们通常所说的跨域是狭义的，是由浏览器同源策略限制的一类请求场景。
什么是同源策略？
同源策略/SOP（Same origin policy）是一种约定，由Netscape公司1995年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到XSS、CSFR等攻击。所谓同源是指"协议+域名+端口"三者相同，即便两个不同的域名指向同一个ip地址，也非同源。

同源策略限制以下几种行为：

1.) Cookie、LocalStorage 和 IndexDB 无法读取
2.) DOM 和 Js对象无法获得
3.) AJAX 请求不能发送


跨域解决方案
1、 通过jsonp跨域
2、 document.domain + iframe跨域
3、 location.hash + iframe
4、 window.name + iframe跨域
5、 postMessage跨域
6、 跨域资源共享（CORS）
7、 nginx代理跨域
8、 nodejs中间件代理跨域
9、 WebSocket协议跨域
```

## Model

### 基表

```python
基表，为抽象表，是专门用来被继承，提供公有字段的，自身不会完成数据库迁移.(abstract)
class BaseModel(models.Model):
    is_delete = models.BooleanField(default=False)
    create_time = models.DateTimeField(auto_now_add=True)

    class Meta:
        # 设置 abstract = True 来声明基表 作为基表的model 不能在数据库中有对应的表
        abstract = False
```

### 模型管理器、自定义模型管理器

```python
每个模型类默认都有一个 objects 类属性，可以把它叫 模型管理器。它由django自动生成，类型为
model表：
class Department(models.Model):
  # 模型管理器
  objects  = models.Manager()
  # 自定义模型管理器
   manager = DepartmentManager()
   
   
 class DepartmentManager(Manager):
 #继承django.db.models.manager.Manager
  # 修改管理器返回的原始查询集
  def all(self):
    """重写all方法：只返回2009年之后成立的部门"""
    return super().all().filter(create_date__gte=date(2009,1,1))
  # 在模型管理器中封装增删查的方法
  def create_dep(self, name, create_date):
    """新增一个部门"""
    dep = Department()
    dep.name = name
    dep.create_date = create_date
    dep.save()
    return dep # 返回新增后的员工对象

```



### 查询

```

```



##  序列化器Serializer

https://www.cnblogs.com/zhuangyl23/p/11901839.html

drf的序列化与反序列化概念

```
导入序列化模块
from rest_framework.serializers import Serializer, ModelSerializer, ListModelSerializer
```

```
序列化与反序列化
序列化: 将对象序列化成字符串用于传输
反序列化: 将字符串反序列化成对象用于使用
```

```
drf的序列化与反序列化
序列化: 将Model类对象序列化成字符串用于传输
反序列化: 将字符串反序列化成Model对象用于使用
```



### 模型类序列化器ModelSerializer、反序列化数据校验

反序列化数据校验

``` python
系统字段校验--局部钩子--全局钩子（优先级：自定义》局部》全局）

系统的字段，可以在Field类型中设置系统校验规则（name=serializers.CharField(min_length=3)）

自定义的反序列字段，设置系统校验规则同系统字段，但是需要在自定义校验规则中（局部、全局钩子）将自定义反序列化字段取出（返回剩余的数据与数据库交互）

is_valid：
证数据：使用序列化器进行反序列化时，需要对数据进行验证后，才能获取验证成功的数据或保存成模型类对象

局部钩子的方法命名 
validate_属性名(self, 属性的value)，校验规则为 成功返回属性的value 失败抛出校验错误的异常
def validate_mobile_phone(self, mobile_phone):
    # 注意参数，self以及字段名
    # 注意函数名写法，validate_ + 字段名字
    if not re.match(REGEX_MOBILE, mobile):
    # REGEX_MOBILE表示手机的正则表达式
        raise serializers.ValidationError("手机号码非法")
    return mobile_phone
    
全局钩子的方法命名 
validate(self, 所有属性attrs)，校验规则为 成功返回attrs 失败抛出校验错误的异常
attrs：系统与局部钩子校验通过的所有数据
    def validate(self, attrs):
    # 传进来什么参数，就返回什么参数，一般情况下用attrs
        if attrs['start'] > attrs['finish']:
            raise serializers.ValidationError("finish must occur after start")
        return attrs
```

```


```



```python
extra_kwargs:参数为ModelSerializer添加或修改原有的选项参数,系统校验规则
extra_kwargs划分只序列化或只反序列化字段（一般我们把需要存入到数据库中的使用write_only（反序列化）,只需要展示的就read_only(序列化)，看需求设计）
extra_kwargs = {
            'name': {
                'required': True,  #设置name字段必填
                'min_length': 1,
                'error_messages': {
                    'required': '必填项',
                    'min_length': '太短',
                },
                'authors': {
                'write_only': True
            },
            'img': {
                'read_only': True,
            },
            }
        }
  write_only：只反序列化
  read_only：只序列化
        
```







### ListModelSerializer