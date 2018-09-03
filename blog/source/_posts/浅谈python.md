---
title: 浅谈python
date: 2018-08-05 14:48:30
tags:
---
## python异常处理 ##
python异常处理使用try，catche，else和finally关键字来尝试可能未成功的操作，处理失败及正常情况以及在事后清理资源

```
try：
	block_try
except Exception 1：  、、第一种except形式
	block_when_exceptin_1_happen:
except (Exception_1 Exception_2 Exception_3): //第二种except形式
	block_when_exception_1_or_2_or_3_happen
except Exception_5 variance:
	block_when_exception_5_happen
except (Exception_6 Exception_7),variance:
	block_when_exception_6_or_7_happen
except :
	block_for_all_other_exceptions
else:
	block_for_no_exception
finally:
	block_anyway
```
##### 自定义异常
自定义异常的边长方法是建立一个继承子系统异常的子类，并且需要引发该异常时用raise语句跑出异常




###### 构造函数
构造函数是一种特殊的类成员方法，主要用在创建对象时初始化对象
python中的构造函数用__init__命名
###### 析构函数
析构函数是构造函数的反函数，在销毁对象时调用他们，往往用来做清理善后，
例如：数据库连接对象可以在析构函数中释放对数据库实例资源的占用
python中为类定义析构函数的方法是在类中定义一个名为__del__的没有返回值和参数的函数
python中显示销毁对象的方法，使用del关键

###### 静态函数和类函数
python中支持两种基于类名访问成员函数：静态函数和类函数
不同点：类函数有一个隐形参数cls可以用来获取类信息
静态函数使用装饰器@staicmethod定义
类函数使用装饰器@classmethod定义

###### 生成器与yield

###### 闭包
###### 装饰器
装饰器是将其他函数增强
1、装饰器本身是一个函数，目的用于装饰其他函数
2、增强被装饰函数的功能
3、装饰器一般接受一个函数对象作为参数以对其进行功能的增强
4、装饰器用@deco定义装饰
