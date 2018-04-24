# django简介

### MTV模式

Django的MTV模式，本质上就是MVC模式，也是为了解耦，只是定义不同

Model：负责业务与数据库（ORM）的对象，对应MVC的Model（数据存取层）

View：负责业务逻辑并适当调用Model和Template，对应MVC的Controller（业务逻辑层）

Template：负责把页面渲染展示给用户，对应MVC的View（表现层）

URL分发器：也叫路由，主要用于将url请求发送给不同的View处理，View在进行相关的业务逻辑处理。

------

### VIRTUALENV虚拟环境创建

本教程使用的python版本为python3.x版本，使用的系统为windows系统

##### virtualenv的安装使用

1. 安装virtualenv

   ```
   pip install virtualenv
   ```

2. 创建虚拟环境

   ```
   virtualenv --no-site-package venv
   ```

   如果有多个python版本，则需要使用-p参数指定python解释器版本

   ```
   virtualenv --no-site-package -p xxx env (xxx为python解释器的路径)
   如 virtualenv --no-site-package -p D:\python3\python.exe env
   ```

3. 进入/退出env

   ```
   进入　cd env/Scripts/文件夹  在activate命令
   退出　deactivate
   ```

------

### DJANGO框架使用指南

##### 创建Django项目

###### 1.首先创建一个运行Django项目的虚拟环境（virtualenv）

虚拟环境的创建参照上面文档，该虚拟环境中要有django库，pymysql库等等所需要的库

大致库的安装命令

```
pip install Django==1.11
pip install PyMySQL
```

###### 2.创建一个Django项目

2.1 创建项目

```
django-admin startproject halloWorld
```

该命令是创建一个名为halloWorld的工程

2.2 运行Django项目

```
python manage.py runserver 端口
```

该命令是运行项目，端口可以不用写，启动的时候会默认随机创建一个可以使用的端口,一般为8000，然后在浏览器输入地址即可访问

###### 3. settings.py配置文件

3.1 设置语言

LANGUAGE_CODE = 'zh-hans' 表示中文 LANGUAGE_CODE = 'en-us' 表示英文

3.2 设置时区

TIME_ZONE = 'Asia/Shanghai'

解释：UTC（世界标准时间），也就是平常说的零时区。 北京时间表示东八区时间，即UTC+8



