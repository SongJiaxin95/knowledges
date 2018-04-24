# Django模型（model）使用指南

> Django中引用了ORM（Objects Relational Mapping）对象关系映射，对不同的数据库都提供了同一调用的API
>
> ORM是一种程序技术，用于实现面向对象编程语言里不同类型系统的数据之间的转换。可以简单理解为翻译机。

#### 修改mysql配置

##### 1. 在settings.py中配置数据库连接信息

```
'ENGINE':'django.db.backends.mysql',
'NAME':'',           #数据库名
'USER':'',           #账号
'PASSWORD':'',       #密码
'HOST':'127.0.0.1',  #IP(本地地址也可以是localhost)
'PORT':'3306',       #端口
```

##### 2. mysql数据库中创建定义的数据库

a. 进入mysql

```
mysql -u root -p
```

b. 创建数据库

```
create database xxx charset=utf-8;
```

##### 3. 配置数据库链接

a. 安装pymysql

```
pip install pymysql
```

b. 在工程目录下的__init__.py文件中输入,完成数据库的驱动加载

```
import pymysql
pymysql.install_as_MySQLdb()
```

##### 4. 定义模型

###### 重要概念：模型，表，属性，字段

一个模型类在数据库中对应一张表，在模型类中定义的属性，对应模型对照表中的一个字段

###### 创建学生模型类

class Student(models.Model):

```
# id = models.AutoField(primary_key=True)  (id 不用写，django会自动生成一个id属性)
stu_name = models.CharField(max_length=10)
stu_age = models.IntegerField()
stu_sex = models.BooleanField(default=1)
stu_birth = models.DateField()
stu_create_time = models.DateField(auto_now_add=True)
stu_operate_time = models.DateField(auto_now=True)
stu_tel = models.CharField(max_length=11)
stu_yuwen = models.DecimalField(max_digits=3,decimal_places=1,default=0)
stu_shuxue = models.DecimalField(max_digits=3, decimal_places=1, default=0)
stu_gid = models.IntegerField(null=True, blank=True)
class Meta:
    db_table = 'cd_student'
    ordering =[]
对象的默认排序字段，获取对象列表时使用，升序ordering['id']，降序ordering['-id']
```

###### 模型字段类型：

```
CharField（max_length） 字符串
	max_length:长度

BooleanField：布尔类型

DateField: 年月日，日期
DateTimeField：年月日 时分秒
	auto_now_add： 第一次创建的时候赋值
	auto_now：每次修改的时候赋值

AutoField：自动增长

IntegerField 整数

FloatField 浮点数

FileField() 文件

ImageField(upload_to=)  upload_to='' 指定上传图片的路径

models.TextField 存文本信息

models.DecimalField(max_digits=3, decimal_places=1) 固定精度
max_digits 总位数 decimal_places 小数后几位
```

###### 模型参数

```
default: 默认
null ：设置是否为空，针对数据库中该字段是否可以为空
blank ：设置是否为空，针对表单提交该字段是否可以为空
primary_key：创建主键
unique：唯一
```

##### 5.迁移数据库

a. 生成迁移文件

```
python manage.py makemigrations
```

注意：如果执行后并没有生成迁移文件，一直提示No changes detected这个结果的话，就要手动的去处理了。有两点处理方式： 1） 先删除掉__pycache__文件夹 2） 直接强制的去执行迁移命令，python manage.py makemigrations xxx (xxx就是app的名称) 3） 查看自动生成的数据库，查看表django_migrations，删掉app字段为xxx的数据(xxx就是app的名称)

b. 执行迁移生成数据库

```
python manage.py migrate
```

注意: 生成迁移文件的时候，并没有在数据库中生成对应的表，而是执行migrate命令之后才会在数据库中生成表

执行命令显示全部ok后，此时在数据库中就生成了表了

------

#### 模型操作

###### 模型成员objects

Django默认通过模型的object对象实现模型数据查询

##### 1. 模型查询

###### 过滤器

查询集表示从数据库获取的对象集合

查询集可以有多个过滤器

过滤器就是一个函数，基于所给的参数限制查询的结果

从SQL角度来说，查询集合和select语句等价，过滤器就像where条件

Django有两种过滤器用于筛选记录 filter	: 返回符合筛选条件的数据集 exclude : 返回不符合筛选条件的数据集

多个filter和exclude可以连接在一起查询

当然还有如下这些过滤器: all() 返回所有数据 filter()	返回符合条件的数据 exclude() 过滤掉符合条件的数据 order_by() 排序 values() 一条数据就是一个字典，返回一个列表

###### 1. 查询单个数据

get()：返回一个满足条件的对象。如果没有返回符合条件的对象，会应该模型类DoesNotExist异常，如果找到多个，会引发模型类MultiObjectsReturned异常

first()：返回查询集中的第一个对象

last()：返回查询集中的最后一个对象

count()：返回当前查询集中的对象个数

exists()：判断查询集中是否有数据，如果有数据返回True，没有返回False

###### 2. 限制查询集

限制查询集，可以使用下表的方法进行限制，等同于sql中的limit

模型名.objects.all()[0:5] 小标不能为负数

###### 3. 字段查询

对sql中的where实现，作为方法，filter(),exclude()，get()的参数

语法：属性名称__比较运算符 = 值

外键：属性名_id

注意：like语句中使用%表示通配符。比如sql语句查询 where name like '%xxx%'，等同于filter(name_contains='xxx')

###### 4. 比较运算符

contains：是否包含，大小写敏感 startswith，endswith：以values开头或者结尾，大小写敏感 以上的运算符前加上i(ignore)就不区分大小写了

isnull，isnotnull：是否为空。filter(name__isnull=True)

in：是否包含在范围内。filter(id__in=[1,2,3])

gt，gte，lt，lte：大于，大于等于，小于，小于等于。filter(age__gt=10)

pk：代表主键，也就是id。filter(pk=1)

###### 5. 聚合函数

ggregate()函数返回聚合函数的值

Avg：平均值

Count：数量

Max：最大

Min：最小

Sum：求和

例如: Student.objects.aggregate(Max('age'))

###### 6. F对象/Q对象

F对象:可以使用模型的A属性与B属性进行比较 背景:在模型中有两个字段，分别表示学生成绩A与成绩B，要对成绩AB进行比较计算，就需要使用到F对象。 例如有如下例子，班级中有女生个数字段以及男生个数字段，统计女生数大于男生数的班级可以如下操作: grades = Grade.objects.filter(girlnum__gt=F('boynum'))

F对象支持算数运算

grades = Grade.objects.filter(girlnum__gt=F('boynum') + 10)

Q对象: Q()对象就是为了将过滤条件组合起来

当我们在查询的条件中需要组合条件时(例如两个条件“且”或者“或”)时。我们可以使用Q()查询对象

使用符号&或者|将多个Q()对象组合起来传递给filter()，exclude()，get()等函数

Q()对象的前面使用字符“~”来代表意义“非”

例如查询，学生中不是12岁的或者姓名叫张三的学生，可以如下操作: student = Student.objects.filter(~Q(age=12) | Q(name='张三')) django 模型（model）

