## 一、python异常处理 ##
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

## 二、 一些函数说明
#### 构造函数
构造函数是一种特殊的类成员方法，主要用在创建对象时初始化对象
python中的构造函数用__init__命名
#### 析构函数
析构函数是构造函数的反函数，在销毁对象时调用他们，往往用来做清理善后，
例如：数据库连接对象可以在析构函数中释放对数据库实例资源的占用
python中为类定义析构函数的方法是在类中定义一个名为__del__的没有返回值和参数的函数
python中显示销毁对象的方法，使用del关键

#### 静态函数和类函数
python中支持两种基于类名访问成员函数：静态函数和类函数
不同点：类函数有一个隐形参数cls可以用来获取类信息
静态函数使用装饰器@staicmethod定义
类函数使用装饰器@classmethod定义

## 三 杂项
#### 迭代器
迭代器是可以迭代的对象。使用__iter__和__next__方法构建自己的迭代器。
迭代器在Python中无处不在。 它们优雅地实现在循环，推导，生成器等中，但隐藏在明显的视觉中。
Python中的迭代器只是一个可以迭代的对象。一个将一次返回数据的对象或一个元素。
Python迭代器对象必须实现两个特殊的方法__iter__()和__next__()，统称为迭代器协议，只有实现这两个方法才算是定义了一个自己的迭代器。
如果我们从中获取一个迭代器，那么一个对象被称为iterable。 大多数Python中的内置容器是列表，元组，字符串等都是可迭代的。iter()函数(这又调用__iter__()方法)返回一个迭代器
- 通过Python中的迭代器迭代
使用next()函数来手动遍历迭代器的所有项目。当到达结束，没有更多的数据要返回时，它将会引发StopIteration

```
# define a list
my_list = [4, 7, 0, 3]
# get an iterator using iter()
my_iter = iter(my_list)
## iterate through it using next() 
#prints 4
for i in range(4):
	print(next(my_iter))
```
- 在Python中构建自己的Iterator
构建迭代器在Python中很容易。只需要实现__iter__()和__next__()方法。
__iter__()方法返回迭代器对象本身。如果需要，可以执行一些初始化。__next__()方法必须返回序列中的下一个项目(数据对象)。 在到达结束后，并在随后的调用中它必须引发StopIteration异常。

在这里，我们展示一个例子，在每次迭代中给出下一个2的几次方。 次幂指数从零开始到用户设定的数字。
```
class PowTwo:
    """Class to implement an iterator
    of powers of two"""

    def __init__(self, max = 0):
        self.max = max

    def __iter__(self):
        self.n = 0
        return self

    def __next__(self):
        if self.n <= self.max:
            result = 2 ** self.n
            self.n += 1
            return result
        else:
            raise StopIteration
```
现在可以创建一个迭代器，并通过它迭代如下 -

```
>>> a = PowTwo(4)
>>> i = iter(a)
>>> next(i)
1
>>> next(i)
2
>>> next(i)
4
>>> next(i)
8
>>> next(i)
16
>>> next(i)
Traceback (most recent call last):
...
StopIteratio
```
#### 生成器与yield
在Python中构建迭代器有很多开销; 必须使用__iter__()和__next__()方法实现一个类，跟踪内部状态，当没有值被返回时引发StopIteration异常。Python生成器是创建迭代器的简单方法。上面提到的所有开销都由Python中的生成器自动处理。简单来说，生成器是返回一个可以迭代的对象(迭代器)的函数(一次一个值)。

- 如何在Python中创建生成器？

	 在Python中创建生成器是相当简单的。 它使用yield语句而不是return语句来定义，与正常函数一样简单。
	 如果函数包含至少一个yield语句(它可能包含其他yield或return语句)，那么它将成为一个生成器函数。 yield和return都将从函数返回一些值。
 - 生成器函数与正常函数的差异
1、生成器函数包含一个或多个yield语句。
2、当被调用时，它返回一个对象(迭代器)，但不会立即开始执行。
3、__iter__()和__next__()之类的方法将自动实现。所以可以使用next()迭代项目。
4、一旦函数退让(yields)，该函数将被暂停，并将该控制权交给调用者。
5、局部变量及其状态在连续调用之间被记住。
6、最后，当函数终止时，StopIteration会在进一步的调用时自动引发。

###### 为什么在Python中使用生成器？
1、容易实现
与其迭代器类相比，发生器可以以清晰简洁的方式实现

2、内存高效
返回序列的正常函数将在返回结果之前会在内存中的创建整个序列。如果序列中的项目数量非常大，这可是要消耗内存的。序列的生成器实现是内存友好的，并且是推荐使用的，因为它一次仅产生一个项目。

3、管道生成器
生成器可用于管理一系列操作，下面使用一个例子说明。
假设我们有一个快餐连锁店的日志文件。 日志文件有一列(第4列)，用于跟踪每小时销售的比萨饼数量，我们想算出在5年内销售的总萨饼数量。假设一切都是字符串，不可用的数字标记为“N / A”。 这样做的生成器实现可以如下

```
with open('sells.log') as file:
    pizza_col = (line[3] for line in file)
    per_hour = (int(x) for x in pizza_col if x != 'N/A')
    print("Total pizzas sold = ",sum(per_hour))
```

4、表示无限流
生成器是表示无限数据流的绝佳媒介。 无限流不能存储在内存中，由于生成器一次只能生成一个项目，因此可以表示无限数据流。

#### 闭包
在函数中定义另一个函数称为嵌套函数。
嵌套函数可以访问包围范围内的变量。
在Python中，这些非局部变量只能在默认情况下读取，我们必须将它们显式地声明为非局部变量(使用nonlocal关键字)才能进行修改。

```
def num_a(x):
	def num_b(y):
		print(x**Y)
	return num_b
```

```
c=num_a(3)
c(2)
9
```
1、什么时候闭包
从上面的例子可以看出，当嵌套函数引用其封闭范围内的值时，在Python中有使用了一个闭包。
在Python中创建闭包必须满足的标准将在以下几点
 - 必须有一个嵌套函数(函数内部的函数)。
 - 嵌套函数必须引用封闭函数中定义的值。
 - 闭包函数必须返回嵌套函数。

2、何时使用闭包？
闭包可以避免使用全局值并提供某种形式的数据隐藏。
它还可以提供面向对象的解决问题的解决方案。
当在类中几乎没有方法(大多数情况下是一种方法)时，闭包可以提供一个替代的和更优雅的解决方案。
但是当属性和方法的数量变大时，更好去实现一个类。
这是一个简单的例子，其中闭包可能比定义类和创建对象更为优先。

```
def make_multiplier_of(n):
    def multiplier(x):
        return x * n
    return multiplier

# Multiplier of 3
times3 = make_multiplier_of(3)

# Multiplier of 5
times5 = make_multiplier_of(5)

# Output: 27
print(times3(9))

# Output: 15
print(times5(3))

# Output: 30
print(times5(times3(2)))
```

#### 装饰器
为了了解装饰器，我们首先在Python中了解一些基本的东西。
Python中的一切(是的，甚至是类)都是对象。 我们定义的名称只是绑定到这些对象的标识符。 函数也不例外，它们也是对象(带有属性)。 各种不同的名称可以绑定到同一个功能对象。

函数可以作为参数传递给另一个函数。如果您在Python中使用了map，filter和reduce等功能，那么您就了解了


装饰器是将其他函数增强
1、装饰器本身是一个函数，目的用于装饰其他函数
2、增强被装饰函数的功能
3、装饰器一般接受一个函数对象作为参数以对其进行功能的增强 
4、装饰器用@deco定义装饰

- 用参数装饰函数

```
def works_for_all(func):
    def inner(*args, **kwargs):
        print("I can decorate any function")
        return func(*args, **kwargs)
    return inner
```


- 在Python中链接装饰器
多个装饰器可以在Python中链接。
这就是说，一个函数可以用不同(或相同)装饰器多次装饰。只需将装饰器放置在所需函数之上。

```
def star(func):
    def inner(*args, **kwargs):
        print("*" * 30)
        func(*args, **kwargs)
        print("*" * 30)
    return inner

def percent(func):
    def inner(*args, **kwargs):
        print("%" * 30)
        func(*args, **kwargs)
        print("%" * 30)
    return inner

@star   // 再用这个进行装饰下面的结果
@percent  //先用这个进行装饰下面的函数
def printer(msg):
    print(msg)

printer("Hello")
```
相当于
```
def printer(msg):
    print(msg)
printer = star(percent(printer))
```


## 四、python设计模式

- 模型视图
- 控制器模式
- 单身模式
- 工厂模式
- 生成器模式
- 原型模式
- 门面模式
- 命令模式
- 适配器模式
- 原型模式
- 装饰模式
- 代理模式
- 责任链模式
- 观察者模式
- 状态模式
- 策略模式
- 模板模式
- 享元模式
- 抽象工厂模式
- 面向对象模式
#### 模型视图控制器(MVC)
模型视图控制器(MVC)是最常用的设计模式。 开发人员发现很容易实现这种设计模式。以下是模型视图控制器(MVC)的基本架构 
![在这里插入图片描述](https://img-blog.csdn.net/20181011142208335?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3F4eGhqeQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 单例模式
单例模式将类的实例化限制为一个对象。 它是一种创建模式，只涉及创建方法和指定对象的一个类。它提供了创建实例的全局访问点。

#### 工厂模式
工厂模式属于创建模式列表类别。它提供了创建对象的最佳方法。在工厂模式中，创建对象时不会将逻辑公开给客户端，并使用通用接口引用新创建的对象。
工厂模式使用工厂方法在Python中实现。 当用户调用一个方法时，传入一个字符串，并通过工厂方法实现创建一个新对象，并将此对象作为返回值。 工厂方法中使用的对象类型由通过方法传递的字符串确定。在下面的例子中，每个方法都包含对象作为参数，这是通过工厂方法实现的。