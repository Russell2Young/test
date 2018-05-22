 0024 面向对象 
---

* [magic](#magic)

* [面向对象三要素](#面向对象三要素)

* [python的类](#python的类)

* [类的定义](#类的定义)

* [类对象及类属性](#类对象及类属性)

* [实例化](#实例化)

* [实例对象instance](#实例对象instance)

* [实例变量和类变量](#实例变量和类变量)

* [使用一个函数装饰一个类](#使用一个函数装饰一个类)

* [普通函数、类方法、静态方法及调用](#普通函数-类方法-静态方法及调用)

* [访问控制](#访问控制)

* [补丁](#补丁)

* [属性装饰器](#属性装饰器)

* [对象的销毁](#对象的销毁)

* [__repr__和__str__方法的区别](#__repr__和__str__方法的区别)

* [方法重载](#方法重载)

* [封装](#封装)

* [继承](#继承)

 * [与继承有关的特殊属性和方法](#与继承有关的特殊属性和方法)

 * [继承中的访问控制](#继承中的访问控制)

 * [方法的重写、覆盖override](#方法的重写-覆盖override)

 * [继承中的初始化](#继承中的初始化)

 * [多继承](#多继承)

 * [Mixin类和装饰器的抉择](#mixin类和装饰器的抉择)

 * [练习](#练习)

* [魔术方法](#魔术方法)

 * [魔术方法代码](#魔术方法代码)

 * [属性查看dir](#属性查看dir)

 * [包含全部魔术方法的代码](#包含全部魔术方法的代码)

 * [创建、初始化、销毁](#创建-初始化-销毁)

 * [hash中的__hash__与__eq__](#hash中的__hash__与__eq__)

 * [bool中的__bool__与__len__](#bool中的__bool__与__len__)

 * [可视化](#可视化)

 * [运算符重载](#运算符重载)

 * [容器和大小](#容器和大小)

 * [可调用对象](#可调用对象)

 * [上下文管理](#上下文管理)

 * [上下文管理的安全性](#上下文管理的安全性)

 * [上下文应用场景](#上下文应用场景)

 * [对一个函数实现上下文管理](#对一个函数实现上下文管理)

 * [反射](#反射)

 * [描述器](#描述器)

 * [描述器应用之classmethod staticmethod property](#描述器应用之classmethod-staticmethod-property)

 * [描述器应用之函数参数类型检查](#描述器应用之函数参数类型检查)

 * [其他杂项](#其他杂项)


		

### magic


### 面向对象三要素
==封装==
- 组装：将数据和操作组装到一起
- 隐藏数据：对外只暴露一些接口，通过接口访问对象。比如驾驶员使用汽车，不需要了解汽车的构造细节，只需要知道什么部件怎么驾驶，踩了油门就跑，可以不了解后面的机动原理

==继承==
- 多复用，继承来的就不用自己写了
- 多继承，少修改，OCP(Open-Closed Principle），使用继承来改变，来体现个性
- 继承分为单继承和多继承，python支持多继承


==多态==
- 面向对象编程最灵活的地方，动态绑定


### python的类
### 类的定义
```
class ClassName:
	语句块
```
- 必须使用` class `关键字
- 类名必须是**大驼峰**命名
- 类定义完成后，就产生了一个类对象，绑定到了标识符` ClassName `上

```py?title="类对象的属性"
class MyClass:
    """A example class"""
    x = 'abc'   # 类属性

    def foo(self):  # 类属性foo，也是方法
        return 'My class'


print(MyClass.x)                # abc
print(MyClass.foo)              # <function MyClass.foo at 0x000001F9DB744488> 类的函数属性的地址
print(hex(id(MyClass.foo)))     # 0x1f9db744488
print(MyClass.__doc__)          # A example class
```
### 类对象及类属性
- 类对象：类的定义就会生成一个类对象，它是type的实例，type对象是object的实例
- 类的属性：类中定义的变量和类中定义的方法都是类的属性
- 类变量：上例中x是类`MyClasss`的变量
- foo是方法对象method，不是普通的函数对象function了，它一般要求至少有一个参数。第一个参数可以是self(self只是一个惯用标识符)，这个参数位置就留给了self
- self：当前实例本身

```py?title="注意函数的表示方法和类对象的表示方法的不同"
class MyClass:
    """A example class"""
    x = 'abc'   # 类属性

    def foo(self):  # 类属性foo，也是方法
        return 'My class'


print(MyClass.x)                # abc
print(MyClass.foo)              # <function MyClass.foo at 0x000001F9DB744488>
print(hex(id(MyClass.foo)))     # 0x1f9db744488
print(MyClass.__doc__)          # A example class


def func():
    pass


print(func)                     # <function func at 0x000002771E574400>
```
![enter description here](http://oqwdmy0iu.bkt.clouddn.com/小书匠/1525441263168.jpg)







### 实例化

` a MyClass() ` 
- 实例化：在类对象名称后面加上一个括号，就是调用类的实例化方法，完成实例化。
- 实例化就是真正地创建一个该类的对象(实例)。
- 每次实例化后获得的实例，是不同的实例，即使是使用同样的参数实例化，得到的也是不一样的对象
- python类实例化后，会自动调用` __init__ `方法。这个方法第一个参数必须留给` self `，其他参数随意
- MyClass()实际上调用的是` __init__(self) `方法，可以不定义，如果没有定义，会在实例化后**隐式调用**
- MyClass()执行时，首先会调用` __new__ `方法，创建一个实例，再把这个实例作为参数传递给` __init__(self, name, age) `进行初始化
- 注意：` __init__() `方法不能有返回值，也就是只能是`None`


```py?title="类对象实例化的过程"
class MyClass(object):
    def __new__(cls, *args, **kwargs):
        print('---new----')
        print('class_id: ', id(cls))
        ret = object.__new__(cls)
        print('instance_id: ', id(ret))
        return ret

    def __init__(self):
        print("-------init")

a = MyClass()
print('class_id: ', id(MyClass))
print('instance_id: ', id(a))

# 打印信息
---new----
class_id:  1636772354744
instance_id:  1636765219192
-------init
class_id:  1636772354744
instance_id:  1636765219192

# 执行过程描述
1 调用类对象MyClass的__new__方法，将自己作为参数传递进去，执行object.__new__方法，返回MyClass的一个实例
2 将MyClass的实例传递给__init__方法，给实例的属性设置一些初始值
3 返回此实例的引用。
```

```py?title="__str__的含义"
# 当使用print输出对象的时候，只要自己定义了__str__(self)方法，那么就会打印从在这个方法中return的数据
class MyClass(object):
    def __new__(cls, *args, **kwargs):
        print('---new----')
        print('class_id: ', id(cls))
        ret = object.__new__(cls)
        print('instance_id: ', id(ret))
        return ret

    def __init__(self):
        print("-------init")

    def __str__(self):
        print("----str----")
        return '对象的描述信息'


b = MyClass()
d = b.__str__()     # 打印 ----str----
c = str(b)          # 打印 ----str----
print(c, d)         # 打印 对象的描述信息 对象的描述信息
print(b)				# 隐含执行的是print(str(b)) -->  print(b.__str__())
```


### 实例对象instance
- 类对象实例化后一定会得到一个类的一个对象，就是实例对象
- ` __init__ `方法的第一个参数`self`就是指代某一个实例
- 类实例化后，得到一个实例对象，实例对象会绑定方法，调用方法时采用` instance_name.method() `的方式
- 但是函数签名是` showage(self) `，` self `就是实例对象，python解释器会把方法的调用者作为第一参数self的实参传入
- ` self.name `就是Jerry对象 的name，name是保存在了Jerry对象上，而不是Person类上，所以称为**实例变量**

```py?title="实例对象的实例变量"
class Person:
    """a example class"""

    x = '123'   # 类变量-->类属性

    def __init__(self, name, age):
        self.name = name    # 实例变量，参数通过实例化时传入
        self.age = age      # 实例变量，参数通过实例化时传入
        self.sex = 'male'   # 实例变量，参数在实例化时隐式设定
        print('self in __init__ = {} '.format(id(self)))    # tom实例化时的打印结果 self in __init__ = 2730084188624 

    def show_age(self):     # 类属性foo，也是方法，方法对象method，self这个名字只是一个惯例，可以修改，但是请不要修改，否则影响代码的可读性
        print('{} is {} '.format(self.name, self.age))


tom = Person('Tom', 29)             # 实例化，会调用__init__方法
print('tom = {}'.format(id(tom)))   # tom = 2730084188624
jerry = Person('Jerry', 24)
print(tom.name, jerry.name)         # 'Tom' 'Jerry'

jerry.age += 1      # jerry.age = 24 + 1 = 25
print(jerry.age)    # 25
jerry.show_age()    # Jerry is 25

sig = inspect.signature(Person.show_age)
print(sig)              # (self)
print(tom.__dict__)     # {'age': 29, 'name': 'Tom', 'sex': 'male'}

```






### 实例变量和类变量
- 实例变量是每一个实例自己的变量，是自己独有的；
- 类变量是类的变量，是类的所有实例共享的属性和方法
- ` __name__ `：对象名
- ` __class__ `：对象的类型，即此对象是由哪个类对象实例化出来的
- ` __dict__ `：对象的属性的字典
- ` __qualname__ `：类的限定名

==总结==
- 是类的，也是这个类所有实例的，它的所有实例都可以访问到；
- 是实例的，就是这个实例自己的，通过类访问不到。
- 类变量是属于类的变量，这个类的所有实例可以共享这个变量
- 实例可以动态的给自己增加一个属性。` 实例.__dict__[变量名] `和` 实例.变量名 `都可以访问到
- 实例的同名变量会隐藏类的同名变量，或者说实例变量覆盖了同名的类变量

==实例属性的查找顺序==
1. 实例访问自己的属性时 `实例.属性名 `，会现在自己的实例属性字典`__dict__`中查找,
2. 如果没有，然后通过属性` __class__ `找到自己的类，再去类的`__dict__`中查找
3. 注意如果实例使用` 实例.__dict__[变量名] ` 访问变量，将不会按照上面的查找顺序找变量了，这是指明使用字典的key查找，不是属性查找。
4. 一般来说，类变量使用全大写来命名。


```python
import inspect


class Person:
    """
    a example class
    实例变量是每一个实例自己的变量，是自己独有的；
    类变量是类的变量，是类的所有实例共享的属性和方法
    """

    country = 'China'   # 类变量-->类属性

    def __new__(cls, *args, **kwargs):
        # print('---new----')
        # print('class_id: ', id(cls))
        ret = object.__new__(cls)   # 将类对象Person作为参数传递给 object.__new__方法，返回类对象Person的一个实例
        # print('instance_id: ', id(ret))
        return ret

    def __init__(self, name, age):  # 将上面__new__方法的返回值Person类对象的实例传递给__init__方法，为此实例添加属性
        self.name = name    # 实例变量，参数通过实例化时传入
        self.age = age      # 实例变量，参数通过实例化时传入
        self.sex = 'male'   # 实例变量，参数在实例化时隐式设定
        print('self in __init__ = {} '.format(id(self)))    # tom实例化时的打印结果 self in __init__ = 2730084188624

    def show_age(self):     # 类属性foo，也是方法，方法对象method，self这个名字只是一个惯例，可以修改，但是请不要修改，否则影响代码的可读性
        print('{} is {} '.format(self.name, self.age))

    def __str__(self):
        return '对象的描述信息'


tom = Person('Tom', 29)             # 实例化，会调用__init__方法
print('tom = {}'.format(id(tom)))   # tom = 2730084188624
jerry = Person('Jerry', 24)
print(tom.name, jerry.name)         # 'Tom' 'Jerry'

jerry.age += 1      # jerry.age = 24 + 1 = 25
print(jerry.age)    # 25
jerry.show_age()    # Jerry is 25

sig = inspect.signature(Person.show_age)
print(sig)              # (self)
print(tom.__dict__)     # {'age': 29, 'name': 'Tom', 'sex': 'male'}


print(tom.country)      # HeBei
print(jerry.country)    # HeBei

Person.country = 'HeBei'
print(tom.country)      # China
print(jerry.country)    # China


print('--------------class--------------')
print(Person.__class__) # <class 'type'>
print(Person.__class__.__base__)    # <class 'object'>
print(Person.__dict__)
# {'__dict__': <attribute '__dict__' of 'Person' objects>, 
# '__new__': <staticmethod object at 0x0000021648CEC5F8>, 
# '__weakref__': <attribute '__weakref__' of 'Person' objects>, 
# 'show_age': <function Person.show_age at 0x0000021648ED99D8>, 
# '__str__': <function Person.__str__ at 0x0000021648ED9A60>, 
# '__init__': <function Person.__init__ at 0x0000021648ED9950>, 
# '__doc__': '\n    a example class\n    实例变量是每一个实例自己的变量，是自己独有的；\n    类变量是类的变量，是类的所有实例共享的属性和方法\n    ', 
# 'country': 'HeBei', '__module__': '__main__'}



print('---------instance tom------------')
print(tom.__class__)                    # <class '__main__.Person'>
print(tom.__class__.__base__)           # <class 'object'>
print(tom.__class__.__base__.__class__) # <class 'type'>
print(tom.__dict__)     # {'name': 'Tom', 'sex': 'male', 'age': 29}

```
```python
class Person:
    age = 3
    height = 170

    def __init__(self, name, age=18):
        self.name = name
        self.age = age


tom = Person('Tom')
jerry = Person("Jerry", 20)

Person.age = 30
print(Person.age, tom.age, jerry.age)     # 30 18 20

print(Person.height, tom.height, jerry.height)  # 170 170 170
jerry.height = 175
print(Person.height, tom.height, jerry.height)  # 170 170 175

tom.height += 10
print(Person.height, tom.height, jerry.height)  # 170 180 175

Person.height += 15
print(Person.height, tom.height, jerry.height)  # 185 180 175

Person.weight = 70
print(Person.weight, tom.weight, jerry.weight)  # 70 70 70

print(tom.__dict__['height'])
# print(tom.__dict__['weight']) # 这是类的属性字典中的item


```
### 使用一个函数装饰一个类
```python
def add_name(name):
    def wrapper(cls):
        cls.name = name
        return cls
    return wrapper


@add_name('sunshine')
class Person:
    age = 13


print(Person.__dict__['name'])


```
### 普通函数、类方法、静态方法及调用
1. 普通函数
2. 类方法
	- 在类中定义的方法，使用` @classmethod `装饰器修饰	
	- 必须至少有一个参数，且第一个参数留给了` cls `，` cls `指代调用者，即 类对象本身
	- ` cls `这个标识符可以是任意合法名称，但是为了易读，请不要修改
	- 通过 `cls`可以直接操作类的属性，但是无法通过`cls`操作类的实例
	- 类对象本身和类对象的实例都可以调用类方法
3. 静态方法
	- 在类中定义，使用` @staticmethod `装饰器修饰的方法
	- 调用时不会隐式的传入参数
	- 类对象和类对象的实例都可以调用静态方法
	- 静态方法，只是表明这个方法属于这个名词空间。函数归在一起，方便组织管理
4. 方法的调用
	- 类几乎可以调用所有内部定义的方法，但是调用 `普通的方法` 时会报错，原因是第一参数必须是类的实例
	- 实例也几乎可以调用所有的方法，` 普通的函数 `的调用一般不可能出现，因为不允许这么定义
	- 类除了普通方法都可以调用，普通方法需要对象的实例作为第一参数
	- 实例可以调用所有类中定义的方法(包括类方法、静态方法)，普通方法传入实例自身，静态方法和类方法需要找到实例的类
	
```python

class Person:
    """
    a example class
    实例变量是每一个实例自己的变量，是自己独有的；
    类变量是类的变量，是类的所有实例共享的属性和方法
    """

    country = 'China'   # 类变量-->类属性

    def __new__(cls, *args, **kwargs):
        # print('---new----')
        # print('class_id: ', id(cls))
        ret = object.__new__(cls)   # 将类对象Person作为参数传递给 object.__new__方法，返回类对象Person的一个实例
        print('instance_id: ', id(ret))
        return ret

    def __init__(self, name, age, job='IT'):  # 将上面__new__方法的返回值Person类对象的实例传递给__init__方法，为此实例添加属性
        self.name = name    # 实例变量，参数通过实例化时传入
        self.age = age      # 实例变量，参数通过实例化时传入
        self.sex = 'male'   # 实例变量，参数在实例化时隐式设定
        self.job = job
        print('self in __init__ = {} '.format(id(self)))    # tom实例化时的打印结果 self in __init__ = 2730084188624

    def show_age(self):     # 类属性foo，也是方法，方法对象method，self这个名字只是一个惯例，可以修改，但是请不要修改，否则影响代码的可读性
        print('{} is {} '.format(self.name, self.age))

    def __str__(self):
        return '对象的描述信息'

    # 普通函数
    # def normal_method():    # 可以定义，但是没有同行这么使用，可以说禁止使用
    #     print("normal_method")


    @classmethod
    def class_method(cls):
        print("class = {0.__name__} ({0})".format(cls))
        cls.HEIGHT = 170


    @staticmethod
    def static_method():
        print(Person.HEIGHT)


Person.class_method()               # class = Person (<class '__main__.Person'>)
Person('sun', 12).class_method()    # class = Person (<class '__main__.Person'>)
# 以上说明，类对象和类对象的实例都可以调用类方法

Person.static_method()
Person('sun', 12).static_method()

# class = Person (<class '__main__.Person'>)
# instance_id:  1245871791576
# self in __init__ = 1245871791576
# class = Person (<class '__main__.Person'>)
# 170
# instance_id:  1245871791576
# self in __init__ = 1245871791576
# 170
# 这里两次__new__返回的类对象实例的内存地址相同可能是没有引用导致的


a = Person('ww', 23)
b = Person('ee', 34)
print(a is b)
print(a == b)
# instance_id:  2507748722448
# self in __init__ = 2507748722448
# instance_id:  2507749900128
# self in __init__ = 2507749900128
# False
# False
# 以上结果说明，两次__new__返回的实例对象都是新开辟的内存地址



print('-----类访问')
print(1, Person.normal_function())  # 普通函数可调用
# print(2, Person.show_age())       # 普通方法不可调用
print(3, Person.class_method())     # 类方法可调用
print(4, Person.static_method())    # 静态方法可调用
# print(Person.__dict__)

tom = Person('Tom', 13)
print('-------实例访问')               
# print(1, tom.normal_function())   # 普通函数不可调用
print(2, tom.show_age())            # 普通方法可调用
print(3, tom.class_method())        # 类方法可调用
print(4, tom.static_method())       # 静态方法可调用
```


### 访问控制
- 私有属性：使用双下划线开头的属性名，·__变量名·
- 私有变量的本质：python解释器将以`__变量名`更改为` _类名__变量名 `
- 保护变量：在变量名前使用一个下划线开头，`_变量名`，解释器对此不做任何处理，只是开发者共同的约定，看见这种变量，就如同私有变量，不要直接使用
- 私有方法：使用双下划线开头的方法，`__method`
- 保护方法：使用单下划线开头的方法，`_method`


==私有方法的本质==
- 单下划线的方法只是开发者之间的约定，解释器不做任何改变
- 双下划线的方法，是私有方法，解释器会改名，改名策略和私有变量相同，`_类名__方法名`
- 方法变量都在类的`__dict__`中可以找到


==私有成员的总结==
- 在python中使用单下划线`_`或双下划线`__`来标识一个成员被保护或者被私有化隐藏起来
- 但是不管什么样的访问控制，都不能真正的阻止用户修改类的成员。Python中没有绝对的安全的保护成员或者私有成员
- 因此，前导的下划线只是一种警告或提醒，请遵守这个约定。除非真有必有，不要修改或者使用保护成员或者私有成员，更不要修改它们
- 
### 补丁
- 可以通过修改或者替换类的成员。使用者调用的方式每有改变，但是类提供的功能可能已经改变了
- 猴子补丁(Monkey Patch)
- 在运行时，对属性、方法、函数等进行动态替换。其目的往往是为了通过替换、修改来增强、扩展原有代码的能力
- 注意：此为黑魔法，慎用
- **猴子补丁的使用场景之一**：假设Person类get_score方法是从数据库拿数据，但是测试的时候，不方便。使用猴子补丁，替换了get_score方法，返回模拟的数据

```py?title="猴子补丁使用示例"
class Person:
    def get_score(self):    # 类中原有的方法
        ret = {'englist': 78, 'chinese': 86, 'history': 43}
        return ret


def get_score(self):        # 替换的目标函数
    return dict(name=self.__class__.__name__, english=88, chinese=90, history=85)


def monkeypatch4Person():   # 执行替换操作的函数
    Person.get_score = get_score    # 将Person类的get_score指向的方法体替换为函数get_score的函数体


if __name__ == '__main__':
    print(Person().get_score())     # 未替换之前打印
    monkeypatch4Person()            # 执行替换函数
    print(Person().get_score())     # 替换之后打印的效果

# # =============================打印结果===========================
# {'chinese': 86, 'english': 78, 'history': 43}
# {'chinese': 90, 'english': 88, 'history': 85, 'name': 'Person'}

```










### 属性装饰器
- 注意使用property装饰器的时候，被装饰的三个方法必须同名，而且他们之间是有顺序
- property装饰器：后面跟的函数名就是以后的属性名。他就是getter。这个必须有，有了它至少是只读属性
- setter装饰器：与属性名同名，且接收2个参数，第一个是self，第二个是将要赋值的值，有了它，属性可写
- deleterious装饰器：控制是否删除属性，很少使用
- property装饰器必须在前，setter、deleterious装饰器在后。
- property装饰器能通过简单的方式，把对方法的操作变成对属性的访问，并起到了一定的隐藏效果





```python?title=''
class Person:
    """
    a example class
    实例变量是每一个实例自己的变量，是自己独有的；
    类变量是类的变量，是类的所有实例共享的属性和方法
    """

    country = 'China'   # 类变量-->类属性

    def __new__(cls, *args, **kwargs):
        # print('---new----')
        # print('class_id: ', id(cls))
        ret = object.__new__(cls)   # 将类对象Person作为参数传递给 object.__new__方法，返回类对象Person的一个实例
        # print('instance_id: ', id(ret))
        return ret

    def __init__(self, name, age, job='IT'):  # 将上面__new__方法的返回值Person类对象的实例传递给__init__方法，为此实例添加属性
        self.name = name    # 实例变量，参数通过实例化时传入
        self.age = age      # 实例变量，参数通过实例化时传入
        self.sex = 'male'   # 实例变量，参数在实例化时隐式设定
        self.__job = job
        # print('self in __init__ = {} '.format(id(self)))    # tom实例化时的打印结果 self in __init__ = 2730084188624

    # 普通方法
    def show_age(self):     # 类属性foo，也是方法，方法对象method，self这个名字只是一个惯例，可以修改，但是请不要修改，否则影响代码的可读性
        print('{} is {} '.format(self.name, self.age))

    def __str__(self):
        return '对象的描述信息'

    # 普通函数
    def normal_function():    # 可以定义，但是没有同行这么使用，可以说禁止使用
        print("normal_function")

    # 类方法
    @classmethod
    def class_method(cls):
        print("class = {0.__name__} ({0})".format(cls))
        cls.HEIGHT = 170

    # 静态方法
    @staticmethod
    def static_method():
        print(Person.HEIGHT)

    # 只读属性
    @property
    def job(self):
        return self.__job

    # 可写属性
    @job.setter
    def job(self, job):
        self.__job = job

    # 删除属性
    @job.deleter
    def job(self):
        # del self.__job
        print("属性删除")


sun = Person('sun', 12)     # 类对象实例化
print(sun.job)              # 读取类对象实例的job属性
sun.job = 'ma'              # 对类对象实例的job属性值进行设置
print(sun.job)              # 再次读取类对象实例的job属性
del sun.job                 # 删除类对象实例的job属性
```

```py?title="属性保护的几种书写方式"
class Person:
    """
    a example class
    实例变量是每一个实例自己的变量，是自己独有的；
    类变量是类的变量，是类的所有实例共享的属性和方法
    """

    country = 'China'  # 类变量-->类属性

    def __new__(cls, *args, **kwargs):
        # print('---new----')
        # print('class_id: ', id(cls))
        ret = object.__new__(cls)  # 将类对象Person作为参数传递给 object.__new__方法，返回类对象Person的一个实例
        # print('instance_id: ', id(ret))
        return ret

    def __init__(self, name, age, job='IT'):  # 将上面__new__方法的返回值Person类对象的实例传递给__init__方法，为此实例添加属性
        self.name = name  # 实例变量，参数通过实例化时传入
        self.age = age  # 实例变量，参数通过实例化时传入
        self.sex = 'male'  # 实例变量，参数在实例化时隐式设定
        self.__job = job
        # print('self in __init__ = {} '.format(id(self)))    # tom实例化时的打印结果 self in __init__ = 2730084188624

    # 普通方法
    def show_age(self):  # 类属性foo，也是方法，方法对象method，self这个名字只是一个惯例，可以修改，但是请不要修改，否则影响代码的可读性
        print('{} is {} '.format(self.name, self.age))

    def __str__(self):
        return '对象的描述信息'

    # 普通函数
    def normal_function():  # 可以定义，但是没有同行这么使用，可以说禁止使用
        print("normal_function")

    # 类方法
    @classmethod
    def class_method(cls):
        print("class = {0.__name__} ({0})".format(cls))
        cls.HEIGHT = 170

    # 静态方法
    @staticmethod
    def static_method():
        print(Person.HEIGHT)

    # =============================================属性保护_v1
    # # 只读属性
    # @property
    # def job(self):
    #     return self.__job
    #
    # # 可写属性
    # @job.setter
    # def job(self, job):
    #     self.__job = job
    #
    # # 删除属性
    # @job.deleter
    # def job(self):
    #     # del self.__job
    #     print("属性删除")

    # =====================================属性保护_v2
    # def get_job(self):
    #     return self.__job
    #
    # def set_job(self, job):
    #     self.__job = job
    #
    # def del_job(self):
    #     # del self.__job
    #     print('删除属性')
    #
    # job = property(get_job, set_job, del_job, 'job property')

    # =====================================属性保护_v3
    job = property(lambda self:self.__job)


sun = Person('sun', 12)  # 类对象实例化
print(sun.job)  # 读取类对象实例的job属性
```
### 对象的销毁
- 类中可以定义 ` __del__ `方法，称为析构函数(方法)
- 作用：销毁类的实例的时候调用，以释放占用的资源。其中就写一些清理资源的代码，比如释放连接
- 注意逐个方法不能引起对象的真正销毁，只是对象销毁的时候回自动调用它。
- 使用` del `语句删除实例，引用计数减一。当引用计数为0时，会自动调用 `__del__`方法
- 用于python实现了垃圾回收机制，不能确定对象何时执行垃圾回收。
- 由于垃圾回收对象销毁时，才会真正清理对象，还会在之前自动调用 `__del__` 方法，除非你明确知道自己的目的，建议不要手动调用这个方法
```py?title="析构方法"
# #===============================================通过del内置函数减少引用计数=================
class Person:

    def __init__(self, name, age):
        self.name = name
        self.__age = age

    def __del__(self):
        print(self.name,"__del__")


if __name__ == "__main__":
    p1 = Person('dog', 12)
    p3 = Person('cat', 12)
    print('-----')
    p2 = p1                 # 'dog'对应的实例的引用计数为2，分别被p2 p1引用
    del p1                  # 删除p1和'dog'之间的关系，并不会执行__del__方法，引用计数减一
    p2.__del__()            # 每次执行该方法，都会执行此方法下的函数体
    p2.__del__()
    print('+++++')
    del p2                  # 删除p2和'dog'之间的关系，引用计数为0，此时会自动执行__del__方法
    print('----------')
    del p3                  # 删除p3和'cat'之间的关系，引用计数为0，此时会自动执行__del__方法
    print('++++++++++')

# # 执行结果
# -----
# dog __del__
# dog __del__
# +++++
# dog __del__
# ----------
# cat __del__
# ++++++++++



# # ========================================================程序结束时，自动调用__del__方法，执行对象销毁=====================

class Person:

    def __init__(self, name, age):
        self.name = name
        self.__age = age

    def __del__(self):
        print(self.name,"__del__")


if __name__ == "__main__":
    p1 = Person('dog', 12)
    p3 = Person('cat', 12)
    
    
# 程序结束自动调用__del__方法
# dog __del__
# cat __del__



```

### __repr__和__str__方法的区别


`__repr__`和`__str__`这两个方法都是用于显示的，`__str__`是面向用户的，而`__repr__`是面向程序员的
-  打印操作会首先尝试__str__和str内置函数，它通常应该返回一个友好的显示
-  `__repr__`用于所有其他的环境中：用于交互模式下提示信息 或者 `repr`函数
-  如果我们想所有环境下都统一显示时，可以重构`__repr__`方法
-  当我们想在不同环境下支持不同的显示，例如终端用户显示使用__str__，而程序员在开发期间则使用底层的__repr__来显示，
-  实际上__str__只是覆盖了__repr__以得到更友好的用户显示


```python
In [1]: class Test:
   ...:     def __init__(self,value='hello_world'):
   ...:         self.data = value
   ...:         

In [2]: t = Test()

In [3]: t
Out[3]: <__main__.Test at 0x7f03c1c36358>

In [4]: print(t)
<__main__.Test object at 0x7f03c1c36358>
# 通过上面的打印结果显示，打印结果不是很友好，显示的是对象 的内存地址


# 重构__repr__
In [5]: class TestRepr(Test):
   ...:     def __repr__(self):
   ...:         return 'TestRepr{}'.format(self.data)
   ...:     

In [6]: tr = TestRepr()

In [7]: tr
Out[7]: TestReprhello_world

In [8]: print(tr)
TestReprhello_world
# 重构__repr__方法后，不管直接输出对象还是通过print打印的信息都按照__repr__方法的结果执行


# 重构__str__
In [9]: class TestStr(Test):
   ...:     def __str__(self):
   ...:         return 'value: {}'.format(self.data)
   ...:     

In [10]: ts = TestStr()

In [11]: ts
Out[11]: <__main__.TestStr at 0x7f03c04c0908>

In [12]: print(ts)
value: hello_world
# 直接输出对象并没有按照__str__方法中定义的格式进行输出，而用print打印的信息则发送改变了


# # 当str和repr都定义时，str会覆盖repr的执行
In [13]: class TestStr(Test):
    ...:     def __repr__(self):
    ...:         return 'repr'
    ...:     def __str__(self):
    ...:         return 'str'
    ...:     

In [14]: tr = TestStr()

In [15]: tr
Out[15]: repr

In [16]: print(tr)
str


```

### 方法重载
- 在其他面向对象的高级语言中，都有重载的概念
- 所谓重载，就是同一个方法名，但是参数数量、类型不一样，就是同一个方法的重载
- python没有重载
- python不需要重载
- python中，方法(函数)定义中，形参非常灵活，不需要指定类型(就算指定了也只是一个说明而非约束)，参数个数也不固定(可变参数)。
- 一个函数的定义可以实现很多种不同形式实参的调用。所以python不需要方法的重载
- 或者说python本身就实现了其他语言的重载




### 封装
- 将数据和操作组织到类中，即属性和方法
- 将数据隐藏起来，给使用者提供操作(方法)。使用者通过操作就可以获取或者修改数据
- getter和setter：通过访问控制，暴露适当的数据和操作给用户，该隐藏的隐藏起来，例如保护成员和私有成员

### 继承
继承：可以让子类从父类获取特征(属性和方法)
父类：又称基类、超类
子类：派生类

==格式：==
```py?title=""
class 子类名(基类1[,基类2,...])
	语句块


class A:
	pass
	
# 等价于
class A(object):
	pass
```
#### 与继承有关的特殊属性和方法
`__base__`：类的基类
`__bases__`：类的基类们
`__mro__`：显示方法查找顺序
` mro() `：方法
` __subclasses__() `：类的子类列表

#### 继承中的访问控制
==分析==
- 从父类继承，自己没有的，就可以到父类中找
- 私有的都是不可访问的，但是本质上依然是改了名称放在这个属性所在类的`__dict__`
- 知道这个新名称就可以直接找到这个隐藏的变量，这是个黑魔法技巧，慎用


==总结==
- 继承时，公有的成员，子类和实例都可以随意访问；
- 私有成员被隐藏，子类和实例不可直接访问，但私有变量所在的类内的方法可以访问这个私有变量
- 属性的查找顺序：实例的`__dict__` >>> 类的` __dict__ ` >>> 如果有继承 >>> 父类的`__dict__`

```py?title=""
class Animal:
    __COUNT = 100
    COUNT = 300
    HEIGHT = 0

    def __init__(self, age, weight, height):
        self.__COUNT += 1
        self.COUNT += 1
        self.age = age
        self.__weight = weight
        self.HEIGHT = height

    def eat(self):
        print("{} eat".format(self.__class__.__name__))

    def __getweight(self):
        print(self.__weight)

    @classmethod
    def showcount1(cls):
        print(cls.__COUNT)

    @classmethod
    def showcount2(cls):
        print(cls.__COUNT)

    def showcount3(self):
        print(self.__COUNT)


class Cat(Animal):
    NAME = 'CAT'
    COUNT = 400
    __COUNT = 200


# c = Cat() # __init__参数错误
c = Cat(3, 5, 15)
print(c.__dict__)   # {'HEIGHT': 15, '_Animal__COUNT': 101, '_Animal__weight': 5, 'age': 3, 'COUNT': 401}
print(c.HEIGHT)     # 15

# print(c.__COUNT)    # 私有属性不可访问
# c.__showcount2()    # 私有方法不可访问

c.showcount3()      # 方法在哪个类中执行，就查找相应类的属性字典

print(c.NAME)       # 先在实例自己的属性字典中查找，再在实例所属的类中查找，再去父类中查找，一级一级向上直到object类中

print("{}".format(Animal.__dict__))

# {'HEIGHT': 0,
# '__init__': <function Animal.__init__ at 0x00000260289E4488>,
# '__module__': '__main__',
# '__dict__': <attribute '__dict__' of 'Animal' objects>,
# 'showcount3': <function Animal.showcount3 at 0x00000260289E49D8>,
# '__weakref__': <attribute '__weakref__' of 'Animal' objects>,
# '__doc__': None, 'COUNT': 300,
# '_Animal__getweight': <function Animal.__getweight at 0x00000260289E4840>,
# '_Animal__COUNT': 100,
# 'showcount1': <classmethod object at 0x00000260289EBA90>,
# 'showcount2': <classmethod object at 0x00000260289EBAC8>,
# 'eat': <function Animal.eat at 0x00000260289E47B8>}

print(Cat.__dict__)
# {'NAME': 'CAT', '__doc__': None, 'COUNT': 400, '_Cat__COUNT': 200, '__module__': '__main__'}


print(c.__dict__)
# {'COUNT': 401, 'age': 3, '_Animal__COUNT': 101, 'HEIGHT': 15, '_Animal__weight': 5}


```


#### 方法的重写、覆盖override
```py?title=""
class Animal:
    def shout(self):
        print("Animal shouts")


class Cat(Animal):
    # 覆盖了父类的方法
    def shout(self):
        print('miao')

a = Animal()

a.shout()  # Animal shouts

c = Cat()

c.shout()  # miao

print(a.__dict__)  # {}

print(c.__dict__)  # {}

print(Animal.__dict__)
# {'__doc__': None, '__dict__': <attribute '__dict__' of 'Animal' objects>,
#  '__weakref__': <attribute '__weakref__' of 'Animal' objects>,
# 'shout': <function Animal.shout at 0x000002447DA14488>,
# '__module__': '__main__'}


print(Cat.__dict__)
# {'__module__': '__main__',
# 'shout': <function Cat.shout at 0x000001F07AE247B8>,
# '__doc__': None}
```

```py?title="通过super访问父类的方法"
class Animal:
    def shout(self):
        print("Animal shouts")


class Cat(Animal):
    # 覆盖了父类的方法
    def shout(self):
        print('miao')

    # 覆盖了自身的方法，显式的调用父类的方法
    def shout(self):
        print(1, super())
        print(2, super(Cat, self))
        super().shout()
        super(Cat, self).shout()    # 访问实例所属的类的父类的方法
        self.__class__.__base__.shout(self)  # 不推荐使用


a = Animal()

a.shout()  # Animal shouts

c = Cat()

c.shout()

# 1 <super: <class 'Cat'>, <Cat object>>
# 2 <super: <class 'Cat'>, <Cat object>>
# Animal shouts
# Animal shouts
# Animal shouts

print(a.__dict__)  # {}
#
print(c.__dict__)  # {}
#
print(Animal.__dict__)

# {'__weakref__': <attribute '__weakref__' of 'Animal' objects>, 
# '__doc__': None, 
# '__dict__': <attribute '__dict__' of 'Animal' objects>, 
# '__module__': '__main__', 
# 'shout': <function Animal.shout at 0x000001398A904488>}

print(Cat.__dict__)
# {'__doc__': None, '__module__': '__main__', 
# 'shout': <function Cat.shout at 0x000001398A904840>}


```

```py?title=""
class Animal:
    @classmethod
    def class_method(cls):
        print("class_method_animal")


    @staticmethod
    def static_method():
        print("static_method_animal")



class Cat(Animal):
    @classmethod
    def class_method(cls):
        print("class_method_cat")

    @staticmethod
    def static_method():
        print("static_method_cat")


c = Cat()
c.class_method()    # class_method_cat
c.static_method()   # static_method_cat

```


#### 继承中的初始化
```py?title=""
class A:
    def __init__(self, a):
        self.a = a


class B(A):
    def __init__(self, b, c):
        self.b = b
        self.c = c

    def printv(self):
        print(self.b)
        # print(self.a)   # 出错


f = B(200, 300)

print(f.__dict__)               # {'c': 300, 'b': 200}
print(f.__class__.__bases__)    # (<class '__main__.A'>,)
f.printv()                      # 200

```
上例代码可知：如果类B中定义时声明继承自类A，则在类B中`__bases__`中是可以看到类A
但是这和是否调用类A的构造方法是两回事。
如果B中调用了A的构造方法，就可以拥有父类的属性了。

```py?title=""
class A:
    def __init__(self, a, d):
        self.a = a
        self.__d = d


class B(A):
    def __init__(self, b, c):
        A.__init__(self, b + c, b - c)
        self.b = b
        self.c = c

    def printv(self):
        print(self.b)
        print(self.a)  


f = B(200, 300)

print(f.__dict__)  # {'a': 500, '_A__d': -100, 'c': 300, 'b': 200}
print(f.__class__.__bases__)  # (<class '__main__.A'>,)
f.printv()  
# 200
# 500

# #==================================说明===
# 作为好的习惯，如果父类定义了__init__方法，就应该在子类的__init__方法中调用它

```


我们应该在子类的__init__方法中，显示地调用父类的__init__方法
```py?title=""
class Animal:
    def __init__(self, age):
        print('Animal init')
        self.age = age

    def show(self):
        print(self.age)


class Cat(Animal):
    def __init__(self, age, weight):
        # 调用父类的__init__方法的顺序决定者show方法的结果

        super().__init__(age)   # 先执行，后被覆盖，建议使用
        print('Cat init')
        self.age = age + 1
        self.weight = weight
        # super().__init__(age)   # 后执行，覆盖实例已定义好的不建议使用


c = Cat(10, 5)

c.show()

```

```py?title=""
class Animal:
    def __init__(self, age):
        print('Animal init')
        self.__age = age

    def show(self):
        print(self.__age)


class Cat(Animal):
    def __init__(self, age, weight):
        # 调用父类的__init__方法的顺序决定者show方法的结果

        super().__init__(age)  # 先执行，后被覆盖，建议使用
        print('Cat init')
        self.__age = age + 1
        self.__weight = weight
        # super().__init__(age)   # 后执行，覆盖实例已定义好的不建议使用


c = Cat(10, 5)

c.show()  # 10
print(c.__dict__)
# {'_Cat__weight': 5, '_Animal__age': 10, '_Cat__age': 11}

# 这里打印的结果为10，原因是在__dict__中，所有的私有属性的名称已改名，
# 并且在调用父类Animal的show方法中的__age会被解释为__Animal__age，
# 因此显示的是10，而不是11

# 结论：这样的设计不好Cat的实例c应该显示自己的属性值更好
# 解决办法：一个原则，自己的私有属性，就应该使用自己的方法读取和修改，不要借助其他类的方法，即使是父类或者派生类的方法。
```

#### 多继承
` OCP原则 `：多用继承、少修改
` 继承的用途 `：增强基类、实现多态
多态：面向对象中，父类、子类通过继承联系在一起，如果可以通过一套方法，就可以实现不同表现，就是多态
多态：同一接口，调用的实例不同，返回的结果不同
多态：继承+覆盖

`python`使用`MRO(method resolution order)`解决基类搜索顺序问题
`MRO`三个搜索算法：解决多继承的二义性
- 经典算法，安装定义从左到右，深度优先策略，2.2之前
- 新式类算法，经典算法的升级，重复的只保留最后一个
- C3算法，在类被创建出来的时候，就计算出一个MRO有序列表。2.3之后，python3唯一支持的算法
- python语法是允许多继承，但是python代码是解释执行的，只有执行到的时候，才能发现错误


#### Mixin类和装饰器的抉择
基类提供的方法不应该具体实现，因为它未必适合子类的功能，子类中需要覆盖重写

文档Document类是其他所有文档类的抽象基类
Word、Pdf类是Document的子类

需求：为Document子类提供打印能力
思路：
1. 在document中提供print方法，在抽象基类中定义假的抽象的方法
	- 适用场景：某些类共有的功能，将这些功能封装到抽象基类中(但是不具体实现)，再让该抽象基类的子类自己提供实现的方法
	- 基类提供的方法不应该具体实现，因为它未必适合子类的打印，子类中需要覆盖重写
	- print算是一种能力--打印功能，不是所有的Document的子类都需要的，所以，从这个角度出发，有些问题
```py?title=""
# # 如果实例想调用某种方法，必须自己实现，否则剖出异常
class Document:
    def __init__(self, content):
        self.content = content

    def print(self):
        raise NotImplementedError("抽象基类没有实现")


class Word(Document):
    def print(self):
        print("打印word")


class Pdf(Document):
    def print(self):
        print("打印pdf")
```
2. 需要打印的子类上增加，如果在现有子类上直接增加，违反了OCP原则，所以应该继承后增加。
	- 适用场景：有些类需要某系功能，而有些却不需要这些功能，通过继承让这些类增加这些功能
```py?title=""
class Document:  # 第三方库，不允许修改
    def __init__(self, content):
        self.content = content


class Printable:
    def print(self):
        print(self.content)


class Word(Document):  # 第三方库，不允许修改
    pass


class Pdf(Document):  # 第三方库，不允许修改
    pass


class PrintableWord(Printable, Word):
    pass


pw = PrintableWord("hello world")
pw.print()
```

3. 装饰器：用装饰器增强一个类，把功能给类附加上去，哪个类需要，就装饰它
	- 优点：简单方便，在需要的地方动态增加，直接使用装饰器 
```py?title=""

def printable(cls):
    def _print(self):
        print(self.content, "装饰器")
    cls.print = _print
    return cls


class Document:  # 第三方库，不允许修改
    def __init__(self, content):
        self.content = content


class Word(Document):  # 第三方库，不允许修改
    pass


class Pdf(Document):  # 第三方库，不允许修改
    pass


# 先继承，后装饰
@printable  # PrintableWord=printable(PrintableWord)
class PrintableWord(Word):
    pass


pw = PrintableWord("hello world")
pw.print()
```

4. Mixin本质上就是多继承实现的，它体现的是一种组合的设计模式
- 在面向对象的设计中，一个复杂的类，往往需要很多功能，而这些功能来自不同的类，这就需要很多的类组合在一起
- 从设计模式的角度来说，多组合，少继承
- Mixin的使用原则
	- Mixin类中不应该显式的出现__init__初始化方法
	- Mixin类通常不能独立工作，因为它是准备混入别的类中的部分功能实现
	- Mixin类的祖先类也应该是Mixin类 
```py?title=""
class Document:  # 第三方库，不允许修改
    def __init__(self, content):
        self.content = content


class Word(Document): pass  # 第三方库，不允许修改


class Pdf(Document): pass  # 第三方库，不允许修改


class PrintableMixin:
    def print(self):
        print(self.content, "Mixin")


# 把要实现的功能，放在继承列表的第一位，覆盖其他的同名的方法
class PrintableWord(PrintableMixin, Word): pass


def printable(cls):
    def _print(self):
        print(self.content, '装饰器')

    cls.print = _print
    return cls


@printable
class PrintablePdf(Word): pass


```

#### 练习
1. Shape基类，要求所有子类都必须提供面积的计算，子类有三角形、矩形、圆
2. 上题中圆类的数据可以序列化
```py?title=""
import math


# 面积的计算使用抽象基类实现
class Shape:
    @property
    def area(self):
        raise NotImplementedError("基类未实现")


class Triangle(Shape):
    def __init__(self, a, b, c):
        self.a = a
        self.b = b
        self.c = c

    @property
    def area(self):
        p = (self.a + self.b + self.c) / 2
        return math.sqrt(p * (p - self.a) * (p - self.b) * (p - self.c))


class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    @property
    def area(self):
        return self.width * self.height


class Circle(Shape):
    def __init__(self, radius):
        self.r = radius

    @property
    def area(self):
        return math.pi * self.r * self.r


shapes = [Triangle(3, 4, 5), Circle(4), Rectangle(2, 3)]

for s in shapes:
    print("the area of {} = {}".format(s.__class__.__name__, s.area))

import json
import msgpack

# 序列化采用Mixin继承的方式实现
class SerializableMixin:
    def dumps(self, t='json'):
        if t == 'json':
            return json.dumps(self.__dict__)
        elif t == 'msgpack':
            return msgpack.packb(self.__dict__)
        else:
            raise NotImplementedError("没有实现的序列化")


class SerializableCircleMixin(SerializableMixin, Circle):
    pass


scm = SerializableCircleMixin(4)
print(scm.area)
print(scm.dumps())

```




### 魔术方法
==分类==
- 创建、初始化、销毁
- hash
- bool
- 可视化
- 运算符重载
- 容器和大小
- 可调用对象
- 上下文管理
- 反射
- 描述器
- 其他杂项
#### 魔术方法代码
```py?title=""
class Person:
    """
    a example class
    实例变量是每一个实例自己的变量，是自己独有的；
    类变量是类的变量，是类的所有实例共享的属性和方法
    """

    country = 'China'  # 类变量-->类属性

    # ========================================实例创建、初始化、销毁=============
    def __new__(cls, *args, **kwargs):  # 对象的创建
        ret = object.__new__(cls)  # 将类对象Person作为参数传递给 object.__new__方法，返回类对象Person的一个实例
        return ret

    def __init__(self, name, age, job='IT'):  # 将上面__new__方法的返回值Person类对象的实例传递给__init__方法，为此实例添加属性
        self.name = name  # 实例变量，参数通过实例化时传入
        self.age = age  # 实例变量，参数通过实例化时传入
        self.sex = 'male'  # 实例变量，参数在实例化时隐式设定
        self.__job = job
        self.items = []

    def __del__(self):
        """
        1，该方法在实例的引用计数为0时才会执行
        2，使用内置函数del 作用于该类的实例时，实际上是执行该实例的__del__方法，
        但是只有该实例的引用计数为0时，此__del__才会生效
        """
        print(self.__name__, "__del__")

    # #=======================================================hash=======
    # # 定义了__hash__方法只是说明，这个类的实例能否作为set或字典的key的可能性，
    # # 至于所有的该类的所有的实例的hash值都相同时，说明了发生了hash冲突，需要借助__eq__判断是否相等
    # 如果定义__hash__=None，则该类的实例不可hash
    def __hash__(self) -> int:
        """
        此方法的返回值必须是整型
        :return: int
        """
        return 1

    def __eq__(self, other) -> bool:
        """
        判断两个实例的值是否相等
        :param other:
        :return: bool
        """
        return self.sex == other.sex

    # # ====================================================bool============
    # `__bool__`：内建函数`bool()`，或者对象放在逻辑表达式的位置，调用这个函数返回布尔值。
    # 如果没有定义`__bool__()`，就找`__len__()`返回长度，非0为真。
    # 如果`__len__`也没有定义，那么所有实例都返回真
    def __bool__(self):
        return False

    def __len__(self):
        return 3

    # # ===================================================可视化=============
    # `__repr__`：内建函数`repr()`对一个对象获取字符串表达。
    # 调用`__repr__`方法返回字符串表达，
    # 如果`__repr__`没有定义，就直接返回object的定义，即显示内存地址信息
    # `__str__`：`str()`函数、内建函数`format()`、`print()`调用，需要返回对象的字符串表达。
    # 如果没有定义，就去调用`__repr__`方法返回字符串表达，
    # 如果`__repr__`没有定义，就直接返回对象的内存地址信息
    # `__bytes__`：`bytes()`函数调用，返回一个对象的bytes表达，及返回bytes对象

    def __repr__(self):
        return "repr: {} {}".format(self.name, self.age)

    def __str__(self):
        return "str: {} {}".format(self.name, self.age)

    def __bytes__(self):
        import json
        return json.dumps(self.__dict__).encode(encoding='utf-8')

    # # ==================================================运算符重载==========
    def __sub__(self, other):
        return self.age - other.age

    def __isub__(self, other):
        """
        此方法实现的是-=，两个实例相减，得到一个新的实例
        """
        return Person(self.name, self - other)

    # # ===================================================容器类方法=========
    # `__len__`：内建函数`len()`，返回对象的长度( >= 0的整数)，
    # 如果把对象当做容器类型看，就如果list或者dict。bool()函数调用的时候，如果没有__bool__方法，则会看__len__方法是否存在，存在则返回，非0为真
    # `__iter__`：迭代容器时，调用，返回一个新的迭代器对象
    # `__contains__`：in成员运算符，如果没有实现，就调用__iter__方法遍历
    # `__getitem__`：实现`self[key]`访问。序列对象，key接受整数为索引，或者切片。
    # 对于set和dict，key为hashable。key不存在则引发keyError异常
    # `__setitem__`：和__getitem__的访问类似，是设置值的方法
    # `__missing__`：字典或子类使用`__getitem__()`调用时，key不存在执行该方法

    def __len__(self):  # 重写了上面bool section中的__len__方法
        return len(self.items)

    def add_item(self, item):
        self.items.append(item)

    def __iter__(self):
        return iter(self.items)

    def __getitem__(self, index):  # 索引访问
        return self.items[index]

    def __setitem__(self, index, value):  # 索引赋值
        self.items[index] = value

    def __missing__(self, key):
        return "{} doesnot exists".format(key)

    # #==========================================可调用对象
    # `__call__`：类中定义一个该方法，实例就可以像函数一样调用
    # 可调用对象：定义一个类，并实例化得到其实例，将实例向函数一样调用
    def __call__(self, *args, **kwargs):
        ret = 0
        for x in args:
            ret += x

        self.ret = ret
        return ret

    # 上下文管理对象，当一个对象同时实现了__enter__和__exit__方法，
    # 它就属于上下文管理的对象
    # __enter__进入与此对象相关的上下文。
    # 如果存在该方法，with语法会把该方法的返回值作为as子句中指定的变量
    # 离开with语句块的时候，调用__exit__方法
    # 上下文应用场景：
    # 1.增强功能：在代码执行的前后增加代码，以增强其功能，类似装饰器的功能
    # 2.资源管理：打开资源需要关闭，例如文件对象，网络连接、数据库连接
    # 3.在执行代码之前，在__enter__做权限的验证
    # todo 将一个函数转换有上下文功能
    def __enter__(self):
        print('enter')

    def __exit__(self, exc_type, exc_val, exc_tb):
        """
        with语句中的如果出现抛出异常和sys.exit()语句，
        此函数依然能够执行
        # 情况1：抛出异常后，__exit__的语句依然能够执行
        # 情况2：执行sys.exit()，退出python解释器，依然能够执行__exit__的语句
        # 情况3：使用kill命令杀死进程情况下不会执行
        """
        print('exit')

    # 反射相关的魔术方法
    # __getattr__
    #       instance.__dict__ -->   instance.__class__.__dict__
    #       -->  继承的祖先类（直到object）的__dict__ -->  找不到 - --> > 调用__getattr__()
    # __setarrt__
    #       实例通过 . 点设置属性，例如self.x = x 就会调用__setattr__，并且此方法会拦截所有的实例的属性的设置，
    #       因此想要将属性添加进去，需手动添加
    # __getattribute__
    #       添加该方法后，该方法会拦截所有的实例获取的属性
    #       根据此方法下的返回值有三种情况
    #           1. 直接返回一个值，这样无论什么属性，返回值都相同
    #           2. 抛出异常，直接进入__getattr__方法
    #           3. return object.__getattribute__(self, item) ,
    #               延续之前的属性字典->类属性字典->object->__getattr__

    def __getattr__(self, item):
        return 'missing {}'.format(item)

    def __getattribute__(self, item):
        return object.__getattribute__(self, item)

    def __setattr__(self, key, value):
        self.__dict__[key] = value

    # 普通方法
    def show_age(self):  # 类属性foo，也是方法，方法对象method，self这个名字只是一个惯例，可以修改，但是请不要修改，否则影响代码的可读性
        print('{} is {} '.format(self.name, self.age))

    # 普通函数
    def normal_function():  # 可以定义，但是没有同行这么使用，可以说禁止使用
        print("normal_function")

    # 类方法
    @classmethod
    def class_method(cls):
        print("class = {0.__name__} ({0})".format(cls))
        cls.HEIGHT = 170

    # 静态方法
    @staticmethod
    def static_method():
        print(Person.HEIGHT)

    # =============================================属性装饰器
    # 只读属性
    @property
    def name(self):
        return self.name

    # 可写属性
    @name.setter
    def name(self, name):
        self.name = name

    # 删除属性
    @name.deleter
    def name(self):
        # del self.__job
        print("属性删除")

    # =============================================属性装饰器_v2
    def get_sex(self):
        return self.sex

    def set_sex(self, sex):
        self.sex = sex

    def del_sex(self):
        # del self.__job
        print('删除属性')

    sex = property(get_sex, set_sex, del_sex, 'sex property')

    # ============================================属性装饰器_v3
    job = property(lambda self: self.__job)


```
#### 属性查看dir
如果`dir([obj])`参数obj包含方法`__dir__(self)`，该方法将被调用。
如果参数obj不包含`__dir__(self)`，该方法将最大限度的收集参数信息
`dir()`对于不同类型的对象具有不同的行为：
- 如果对象是模块对象，返回的列表包含模块的属性名
- 如果对象是类型或类对象，返回的列表包含类的属性名，及它的基类的属性名
- 否则，返回列表包含对象的属性名，它的类的属性名和类的基类的属性名
```py?title=""
import animal
from animal import Animal


class Cat(Animal):
    x = 'cat'
    y = 'abcd'


class Dog(Animal):
    def __dir__(self):
        return ['dog']  # 必须返回可迭代对象


print('------')
print("Current Module's names = {} ".format(sorted(dir())))  # 当前模块名词空间内的属性
# Current Module's names = ['Animal', 'Cat', 'Dog', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__',
# '__name__', '__package__', '__spec__', 'animal']
print("animal Module's names = {} ".format(sorted(dir(animal))))  # 指定模块名词空间内的属性
# animal Module's names = ['Animal', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__',
# '__package__', '__spec__']

print("Animal's dir() = {}".format(sorted(dir(Animal))))
# Animal's dir() = ['__class__', '__delattr__', '__dict__', '__dir__',
# '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__',
# '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__',
# '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__',
# '__setattr__', '__sizeof__', '__str__', '__subclasshook__',
# '__weakref__', 'x']

print("Cat's dir() = {} ".format(dir(Cat)))
# Cat's dir() = ['__class__', '__delattr__', '__dict__', '__dir__',
# '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__',
# '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__',
# '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__',
# '__setattr__', '__sizeof__', '__str__', '__subclasshook__',
# '__weakref__', 'x', 'y']

tom = Cat('tome')
print(sorted(dir(tom)))  # 实例tom的属性、Cat类及所有祖先类的类属性
print(sorted(tom.__dir__())) # 同上

```
#### 包含全部魔术方法的代码
```py?title=""
class Person:
    """
    a example class
    实例变量是每一个实例自己的变量，是自己独有的；
    类变量是类的变量，是类的所有实例共享的属性和方法
    """

    country = 'China'  # 类变量-->类属性

    # ========================================实例创建、初始化、销毁=============
    def __new__(cls, *args, **kwargs):  # 对象的创建
        ret = object.__new__(cls)  # 将类对象Person作为参数传递给 object.__new__方法，返回类对象Person的一个实例
        return ret

    def __init__(self, name, age, job='IT'):  # 将上面__new__方法的返回值Person类对象的实例传递给__init__方法，为此实例添加属性
        self.name = name  # 实例变量，参数通过实例化时传入
        self.age = age  # 实例变量，参数通过实例化时传入
        self.sex = 'male'  # 实例变量，参数在实例化时隐式设定
        self.__job = job
        self.items = []

    def __del__(self):
        """
        1，该方法在实例的引用计数为0时才会执行
        2，使用内置函数del 作用于该类的实例时，实际上是执行该实例的__del__方法，
        但是只有该实例的引用计数为0时，此__del__才会生效
        """
        print(self.__name__, "__del__")

    # #=======================================================hash=======
    # # 定义了__hash__方法只是说明，这个类的实例能否作为set或字典的key的可能性，
    # # 至于所有的该类的所有的实例的hash值都相同时，说明了发生了hash冲突，需要借助__eq__判断是否相等
    # 如果定义__hash__=None，则该类的实例不可hash
    def __hash__(self) -> int:
        """
        此方法的返回值必须是整型
        :return: int
        """
        return 1

    def __eq__(self, other) -> bool:
        """
        判断两个实例的值是否相等
        :param other:
        :return: bool
        """
        return self.sex == other.sex

    # # ====================================================bool============
    # `__bool__`：内建函数`bool()`，或者对象放在逻辑表达式的位置，调用这个函数返回布尔值。
    # 如果没有定义`__bool__()`，就找`__len__()`返回长度，非0为真。
    # 如果`__len__`也没有定义，那么所有实例都返回真
    def __bool__(self):
        return False

    def __len__(self):
        return 3

    # # ===================================================可视化=============
    # `__repr__`：内建函数`repr()`对一个对象获取字符串表达。
    # 调用`__repr__`方法返回字符串表达，
    # 如果`__repr__`没有定义，就直接返回object的定义，即显示内存地址信息
    # `__str__`：`str()`函数、内建函数`format()`、`print()`调用，需要返回对象的字符串表达。
    # 如果没有定义，就去调用`__repr__`方法返回字符串表达，
    # 如果`__repr__`没有定义，就直接返回对象的内存地址信息
    # `__bytes__`：`bytes()`函数调用，返回一个对象的bytes表达，及返回bytes对象

    def __repr__(self):
        return "repr: {} {}".format(self.name, self.age)

    def __str__(self):
        return "str: {} {}".format(self.name, self.age)

    def __bytes__(self):
        import json
        return json.dumps(self.__dict__).encode(encoding='utf-8')

    # # ==================================================运算符重载==========
    def __sub__(self, other):
        return self.age - other.age

    def __isub__(self, other):
        """
        此方法实现的是-=，两个实例相减，得到一个新的实例
        """
        return Person(self.name, self - other)

    # # ===================================================容器类方法=========
    # `__len__`：内建函数`len()`，返回对象的长度( >= 0的整数)，
    # 如果把对象当做容器类型看，就如果list或者dict。bool()函数调用的时候，如果没有__bool__方法，则会看__len__方法是否存在，存在则返回，非0为真
    # `__iter__`：迭代容器时，调用，返回一个新的迭代器对象
    # `__contains__`：in成员运算符，如果没有实现，就调用__iter__方法遍历
    # `__getitem__`：实现`self[key]`访问。序列对象，key接受整数为索引，或者切片。
    # 对于set和dict，key为hashable。key不存在则引发keyError异常
    # `__setitem__`：和__getitem__的访问类似，是设置值的方法
    # `__missing__`：字典或子类使用`__getitem__()`调用时，key不存在执行该方法

    def __len__(self):          # 重写了上面bool section中的__len__方法
        return len(self.items)

    def add_item(self, item):
        self.items.append(item)

    def __iter__(self):
        return iter(self.items)

    def __getitem__(self, index):  # 索引访问
        return self.items[index]

    def __setitem__(self, index, value):  # 索引赋值
        self.items[index] = value

    def __missing__(self, key):
        return "{} doesnot exists".format(key)

    # #==========================================可调用对象
    # `__call__`：类中定义一个该方法，实例就可以像函数一样调用
    # 可调用对象：定义一个类，并实例化得到其实例，将实例向函数一样调用
    def __call__(self, *args, **kwargs):
        ret = 0
        for x in args:
            ret += x

        self.ret = ret
        return ret


    # 普通方法
    def show_age(self):  # 类属性foo，也是方法，方法对象method，self这个名字只是一个惯例，可以修改，但是请不要修改，否则影响代码的可读性
        print('{} is {} '.format(self.name, self.age))

    # 普通函数
    def normal_function():  # 可以定义，但是没有同行这么使用，可以说禁止使用
        print("normal_function")

    # 类方法
    @classmethod
    def class_method(cls):
        print("class = {0.__name__} ({0})".format(cls))
        cls.HEIGHT = 170

    # 静态方法
    @staticmethod
    def static_method():
        print(Person.HEIGHT)

    # =============================================属性装饰器
    # 只读属性
    @property
    def name(self):
        return self.name

    # 可写属性
    @name.setter
    def name(self, name):
        self.name = name

    # 删除属性
    @name.deleter
    def name(self):
        # del self.__job
        print("属性删除")

    # =============================================属性装饰器_v2
    def get_sex(self):
        return self.sex

    def set_sex(self, sex):
        self.sex = sex

    def del_sex(self):
        # del self.__job
        print('删除属性')

    sex = property(get_sex, set_sex, del_sex, 'sex property')

    # ============================================属性装饰器_v3
    job = property(lambda self: self.__job)


```


#### 创建、初始化、销毁
`__new__`
`__init__`
`__del__`
#### hash中的__hash__与__eq__
`__hash__`：内建函数`hash()`调用的返回值，返回一个整数。如果定义这个方法，该类的实例就可hash，
`__eq__`：对应`==`操作符，判断两个对象是否相等，返回bool值
`__hash__`方法只是返回一个hash值作为set的key，但是`去重`，还需要`__eq__`来判断两个对象是否相等
hash值相等，只是hash冲突，不能说明两个对象是相等的。
一般来说，提供`__hash__`方法是为了作为set或者dict的key，所以`去重`要同时提供`__eq__`方法
不可hash对象`isinstance(p1,collections.Hashable)`一定为`False`
源码中有一句`__hash__=None`，也就是如果调用`__hash__()`，相当于`None()`，一定报错
所有类都继承object，而这个类是具有`__hash__`方法的，如果一个类不能被hash，就把`__hash__`设置为`None`
#### bool中的__bool__与__len__
`__bool__`：内建函数`bool()`，或者对象放在逻辑表达式的位置，调用这个函数返回布尔值。
如果没有定义`__bool__()`，就找`__len__()`返回长度，非0为真。
如果`__len__`也没有定义，那么所有实例都返回真
#### 可视化
`__repr__`：内建函数`repr()`对一个对象获取字符串表达。调用`__repr__`方法返回字符串表达，如果`__repr__`也没有定义，就直接返回object的定义，即显示内存地址信息
`__str__`：`str()`函数、内建函数`format()`、`print()`函数调用，需要返回对象的字符串表达。如果没有定义，就去调用`__repr__`方法返回字符串表达，如果`__repr__`没有定义，就直接返回对象的内存地址信息
`__bytes__`：`bytes()`函数调用，返回一个对象的bytes表达，及返回bytes对象

#### 运算符重载
==应用场景==
往往用面向对象实现的类，需要做大量的运算，而运算符是这种运算在数学上最常见的表达方式。
提供运算符重载，比直接提供加法方法要更加适合该领域内使用者的习惯
int类，几乎实现了所有操作符，可以作为参考
一般来说，比较的实现，需要等于或者小于方法也就够了，其他可以不实现
比较运算符：`<  <=  ==  >  >= !=` 对应`__lt__`  `__le__`  `__eq__`  `__gt__`  `__ge__`  `__ne__`
算法运算符：
`+  -  *  /  %  //  **  `对应 `__add__` `__sub__` `__mul__` `__truediv__`  `__mod__` `__floordiv__`  `__pow__` `__divmod__`

`+=  -=  *=  /=  %=  //=  **= `对应`__iadd__` `__isub__` `__imul__` `__itruediv__` `__imod__` `ifloordiv__`  `__ipov__`

#### 容器和大小
`__len__`：内建函数`len()`，返回对象的长度(>=0的整数)，如果把对象当做容器类型看，就如果list或者dict。bool()函数调用的时候，如果没有__bool__方法，则会看__len__方法是否存在，存在则返回，非0为真
`__iter__`：迭代容器时，调用，返回一个新的迭代器对象
`__contains__`：in成员运算符，如果没有实现，就调用__iter__方法遍历
`__getitem__`：实现`self[key]`访问。序列对象，key接受整数为索引，或者切片。对于set和dict，key为hashable。key不存在则引发keyError异常
`__setitem__`：和__getitem__的访问类似，是设置值的方法
`__missing__`：字典或子类使用`__getitem__()`调用时，key不存在执行该方法
#### 可调用对象
`__call__`：类中定义一个该方法，实例就可以像函数一样调用
可调用对象：定义一个类，并实例化得到其实例，将实例向函数一样调用
#### 上下文管理
当一个对象同时实现了`__enter__`和`__exit__`方法，他就属于上下文管理的对象
`__enter__`：进入与此对象相关的上下文。如果存在该方法，with语法会把该方法的返回值作为 ` as `子句中指定的变量上
`__exit__ `：退出与此对象相关的上下文
==总结==
实例化对象的时候，并不会调用enter，进入`with语句块`调用`__enter__`方法，然后执行语句块，最后离开`with语句块`的时候，调用`__exit__`方法
with可以开启一个上下文运行环境，在执行前做一些准备工作，执行后做一些收尾工作

##### 上下文管理的安全性
1. 看看异常对上下文的影响
2. 极端例子：` sys.exit() 作用：退出python解释器`对上下文的影响
3. 在linux系统中，让该py文件处于运行中，在后台执行 `ps aux|grep with.py ;kill 其进程id`
```py?title=""
# cat with.py
import sys
import time
class Point:
    def __init__(self):
        print("init")

    def __enter__(self):
        print("enter")

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("exit")


with Point() as f:
    # raise Exception("error")	 # 情况1：抛出异常后，__exit__的语句依然能够执行
    sys.exit(200)   # 0-255		# 情况2：执行sys.exit()，退出python解释器，依然能够执行__exit__的语句
	time.sleep(40)				 # 情况3：使用kill命令杀死进程情况下不会执行__exit__语句
    print('do something')

```

`__enter__`方法返回值就是上下文中使用的对象，with语法会把它的返回值赋给`as`子句的变量

```py?title="__enter__的返回值作为as子句的变量"
class Point:
    def __init__(self):
        print("init")

    def __enter__(self):
        print("enter")
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("exit")


p = Point()

with p as f:
    print(hex(id(p)))
    print(hex(id(f)))

# 0x21dc7522e48
# 0x21dc7522e48

```

`__enter__方法和__exit__方法的参数`


`__enter__`方法没有其他参数
`__exit__`方法有3个参数：
`__exit__(self, exc_type, exc_val, exc_tb)`
- 这三个参数都与异常有关
- 如果该上下文退出时没有异常，这三个参数都为None
- 如果有异常，参数意义如下
	- `exc_type`：异常类型
	- `ext_value`：异常的值
	- `traceback`：异常的追踪信息
- `__exit__`方法返回一个等效True的值，则压制with语句块中的异常，否则继续抛出异常

```py?title="如果with语句中有抛出异常语句，exit的返回值为True，则压制异常，否则继续抛出异常"
class Point:
    def __init__(self):
        print("init")

    def __enter__(self):
        print("enter")
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print(exc_type)
        print(exc_val)
        print(exc_tb)
        print("exit")
        return 'abc' # 分别在注释和不注释此语句的两种情况下测试

p = Point()

with p as f:
    raise Exception("New error")
    print('do sth')


```
##### 上下文应用场景
1. 增强功能
	- 在代码执行的前后增加代码，以增强其功能，类似装饰器的功能
2. 资源管理
	- 打开了资源需要关闭，例如文件对象、网络连接、数据库连接
3. 权限验证
	- 在执行代码之前，做权限的验证，在`__enter__`中处理

##### 对一个函数实现上下文管理
`contextlib.contextmanager`
- 它是一个装饰器，实现上下文管理，装饰一个函数，而不用像类一样实现`__enter__`和`__exit__`方法。
- 被装饰的函数有要求，必须要有`yield`，也就是这个函数必须返回一个生成器，且只有yield一个值
- 装饰器函数必须接收一个生成器对象作为参数
	- 把yield之前的当做`__enter__`方法执行
	- 把yield之后的当做`__exit__`方法执行
	- 把yield的值作为`__enter__`的返回值 


==总结==
- 如果业务逻辑简单，可以使用函数加`contextlib.contextmanager`装饰器方式
- 如果业务复杂，用类方式加`__enter__`和`__exit__`方法方便



```py?title=""
import contextlib


@contextlib.contextmanager
def foo():
    print('enter')  # 相当于__enter__()
    yield 5  # yield的值只能有一个，作为__enter__方法的返回值
    print('exit')
with foo() as f:
    print("--------", f)

# 说明yield中的值作为__enter__方法的返回值，
# 正常情况下，__enter__和__exit__的语句都会执行


@contextlib.contextmanager
def foo():
    print('enter')  # 相当于__enter__()
    yield 5  # yield的值只能有一个，作为__enter__方法的返回值
    print('exit')
with foo() as f:
	raise Exception()
    print("--------", f)
# 如果with语句块中出现异常，那么__exit__方法的语句块则不会执行
# 如果想要__exit__中的语句块执行，则需要添加异常捕获



import contextlib
import sys
@contextlib.contextmanager
def foo():
    print('enter')  # 相当于__enter__()
    try:
        yield 5  # yield的值只能有一个，作为__enter__方法的返回值
    finally:
        print('exit')

with foo() as f:
    raise Exception()
	# sys.exit()
    print("--------", f)

# 增加try  finally后，无论with语句块中出现的是异常还是系统退出，finally语句块都会执行

```
#### 反射
`运行时`：区别于编译时，指的是程序被加载到内存中执行的时候
`反射`：指的是运行时获取类型定义信息 

在python中，能够通过一个对象，找出其`type` `class` `attribute` `method`的能力，称为反射或自省
具有反射能力的函数有：`type() isinstance()  callable()  dir()  getattr()`

`getattr(object,name,default)`：通过name返回object的属性值。当属性不存在，将使用default返回，如果没有default，则抛出AttributeError。name必须为字符串
`setattr(object,name,value)`：object的属性存在，则覆盖；不存在，新增
`hasattr(object,name)`：判断对象是否有这个名字的属性，name必须为字符串


这种动态增删属性的方式是运行时改变类或实例的方式，但是装饰器或Mixin都是定义时就决定了，因此反射能力更具有更大的灵活性

```py?title=""
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return "Point({},{})".format(self.x, self.y)

    def show(self):
        print(self.x, self.y)


p1 = Point(4, 5)
p2 = Point(14, 15)
print(repr(p1), repr(p2), sep='\n')
print(p1.__dict__)

setattr(p1, 'y', 14)  # 有则覆盖
setattr(p1, 'z', 45)  # 无则添加

print(getattr(p1, '__dict__'))
print(p1.__dict__)

# 动态调用方法
if hasattr(p1, 'show'):
    getattr(p1, 'show')()

# 动态添加方法
if not hasattr(Point, 'add'):
    setattr(Point, 'add', lambda self, other: Point(self.x + other.x, self.y + other.y))

print(Point.add)
print(p1.add)  # 绑定
print(p1.add(p2))

# 为实例增加方法，未绑定
if not hasattr(p1, 'sub'):
    setattr(p1, 'sub', lambda self, other: Point(self.x - other.x, self.y - other.y))

print(p1.sub(p2, p1))   # 实例的函数属性执行时，不会自动将自己作为self传递进去



```

==练习==

``` py?title="通过名称找对应的函数执行"
class Dispatcher:
    def __init__(self):
        self._run()

    def cmd1(self):
        print('i am cmd1')

    def cmd2(self):
        print('i an cmd2')

    def _run(self):
        while 1:
            cmd = input('Plz input a command: ').strip()
            if cmd == 'quit':
                break

            getattr(self, cmd, lambda: print('unknown command {} '.format(cmd)))()

Dispatcher()
```

==反射相关的魔术方法==
- `__getattr__()`
- `__setattr__()`
- `__delattr__()`
- `__getattribute__`


1. `__getattr__()`
`一个类的属性会按照继承关系找，如果找不到，就会执行__getattr__方法，如果没有这个方法，就会抛出AttributeError异常，表示找不到属性`

==属性查找顺序==`instance.__dict__  -->   instance.__class__.__dict__ --->  继承的祖先类（直到object）的__dict__  -->  找不到  --->>  调用__getattr__() `
```py?title="__getattr__魔术方法测试"
class Base:
    n = 0


class Point(Base):
    z = 6
    def __init__(self,x,y):
        self.x = x
        self.y = y

    def show(self):
        print(self.x,self.y)

    def __getattr__(self, item):
        return 'missing {} '.format(item)

p1 = Point(4,5)
print(p1.x)
print(p1.z)
print(p1.n)
print(p1.t)
```

2. `__setattr__()`

`实例通过 . 点设置属性，例如self.x = x 就会调用__setattr__，并且此方法会拦截所有的实例的属性的设置，因此想要将属性添加进去，需手动添加`

```py?title="实例通过 . 点设置属性，例如self.x = x 就会调用__setattr__"
class Base:
    n = 0


class Point(Base):
    z = 6

    def __init__(self, x, y):
        self.x = x
        self.y = y

    def show(self):
        print(self.x, self.y)

    def __getattr__(self, item):
        return 'missing {} '.format(item)

    def __setattr__(self, key, value):
        print('setattr {}={}'.format(key, value))
        # self.__dict__[key] = value


p1 = Point(4, 5)
print(1, p1.x)
print(2, p1.y)
print(3, p1.z)
print(4, p1.n)
print(5, p1.t)
# 打印结果
# setattr x=4
# setattr y=5
# 1 missing x
# 2 missing y
# 3 6
# 4 0
# 5 missing t


```



3. `__delattr__()`


```py?title="阻止实例属性的删除"
class Base:
    n = 0


class Point(Base):
    Z = 6

    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __delattr__(self, item):
        print('can not del {}'.format(item))


p = Point(14, 5)

del p.x
p.z = 15
del p.z
del p.Z
print(Point.__dict__)
print(p.__dict__)
del Point.Z
print(Point.__dict__)
```

4. `__getattribute__`

```py?title=""
class Base:
    n = 0


class Point(Base):
    z = 6

    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __getattr__(self, item):
        return 'missing {}'.format(item)

    def __getattribute__(self, item):
        # return item
        # return self.__dict__[item]  # 造成递归错误
        # raise AttributeError('Not Found') # 抛出异常后，直接查找__getattr__方法
        return object.__getattribute__(self, item)  
        # 当返回此项时，就会延续之前的查找顺序 
        # instance.__dict__ -> instance.__class__.__dict__ --> object.__dict__ 
        # --> __getattr__


p1 = Point(4, 5)
print(p1.__dict__)
print(p1.x)
print(p1.y)
print(p1.z)
print(p1.n)
print(p1.t)
print(Point.z)

```


#### 描述器
==描述器的表现==
用到3个魔术方法：`__get__`，`__set__`，`__delete__`
方法签名如下
`object.__get__(self,instance,owner)`
`object.__set__(self,instance,value)`
`object.__delete__(self,instance)`
`self`指代当前实例、调用者
`instance`是`owner`的实例
`owner`是属性的所属的类

```py?title="程序执行流程及赋值即定义的深刻体现"
class A:
    def __init__(self):
        self.a1 = 'a1'
        print('A init')


class B:
    x = A()

    def __init__(self):
        print('B.init')

print('-'*20)
print(B.x.a1)

print('='*20)
b = B()
print(b.x.a1)

# 运行结果
# A init
# --------------------
# a1
# ====================
# B.init
# a1

# 1. 类定义时，类变量需要先生成，而类B的x属性是类A的实例，
# 所以类A先初始化，所以打印A.init
# 2. 然后执行打印B.x.a1
# 3. 类对象B实例化，得到实例b
# 4. 打印b.x.a1,会查找类属性b.x，指向A的实例，
# 所以返回A实例的属性a1的值
```

==描述器定义==
1. 首先类属性是外面类的实例
2. 外面类中实现了`__get__`，`__set__`，`__delete__`三个方法中的任何一个方法
3. 那么这个外面类就是描述器
	- 如果仅实现了`__get__`方法，就是非数据描述器non-data descriptor
	- 如果实现了`__get__  __set__`两个方法，就是数据描述器data descriptor
	- 如果一个类的类属性设置为描述器的实例，那么它就被称为owner属主

```py?title=""
class A:
    def __init__(self):
        self.a1 = 'a1'
        print('A init')

    def __get__(self, instance, owner):
        print("A.__get__ {} {} {}".format(self, instance, owner))
        return self


class B:
    x = A()

    def __init__(self):
        print('B.init')


print('-' * 20)
b = B()
print(B.x)	#因为__get__参数与instance  owner都有关，所有两个都可调用
print(b.x)

# A init
# --------------------
# B.init
# A.__get__ <__main__.A object at 0x000001BECD462E48> None <class '__main__.B'>
# <__main__.A object at 0x000001BECD462E48>
# A.__get__ <__main__.A object at 0x000001BECD462E48> <__main__.B object at 0x000001BECD462F28> <class '__main__.B'>
# <__main__.A object at 0x000001BECD462E48>

```


```py?title="实例的属性名与类属性同名引发的问题"
class A:
    def __init__(self):
        self.a1 = 'a1'
        print('A init')

    def __get__(self, instance, owner):
        print("A.__get__ {} {} {}".format(self, instance, owner))
        return self

    def __set__(self, instance, value):
        print('A.__set__ {} {} {}'.format(self, instance, value))
        self.data = value


class B:
    x = A()

    def __init__(self):
        print('B.init')
        self.x = 'b.x'
        self.y = 'b.y'



b = B()

print(b.__dict__)

# 运行结果
A init
B.init
A.__set__ <__main__.A object at 0x0000029C8D2D2E48> <__main__.B object at 0x0000029C8D2D2F60> b.x
{'y': 'b.y'}

# 从结果分析
1. 如果在为实例添加属性时，实例的属性名与类的属性值为描述器实例的属性名同名，
2. 那么，调用此方法则会调用描述器的__set__方法
3. 如果实例的属性名 与 类的属性值为描述器实例的属性名 不相同，那么该属性则添加至实例的属性字典。



```


#### 描述器应用之classmethod staticmethod   property
```py?title=""


```





#### 描述器应用之函数参数类型检查
```py?title="在类内部进行函数检查"
import inspect


class Person:
    def __init__(self, name: str, age: int):
        sig = inspect.signature(Person)
        params = zip((name, age), [sig.parameters[i].annotation for i in sig.parameters])
        if not self.check_type(params):
            raise Exception("参数不匹配")
        self.name = name
        self.age = age

    def check_type(self, params):
        for i, v in params:
            if isinstance(i, v):
                return True


p1 = Person('sun', 2)
print(p1.__dict__)
```
```py?title="描述器版+侵入式代码"

class CheckType:
    def __init__(self, type, identifier):
        self.type = type
        self.identifier = identifier

    def __get__(self, instance, owner):
        return instance.__dict__[self.identifier]

    def __set__(self, instance, value):
        if isinstance(value, self.type):
            instance.__dict__[self.identifier] = value
            # setattr(instance, self.identifier, value) # 等价于self.name = name，因此触发递归
        else:
            raise Exception('类型不匹配')

class Person:
    name = CheckType(str, 'name')
    age = CheckType(int, 'age')

    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

    def show(self):
        return self.name, self.age


p1 = Person('su', 23)
print(p1.__dict__)
print(p1.show())
p2 = Person('su', '23')
```
```py?title=""述器版+函数装饰器"

import inspect


class CheckType:
    def __init__(self, tpe, identifier):
        self.type = tpe
        self.identifier = identifier

    def __get__(self, instance, owner):
        return instance.__dict__[self.identifier]

    def __set__(self, instance, value):
        if not isinstance(value, self.type):
            raise Exception
        instance.__dict__[self.identifier] = value
        # setattr(instance, self.identifier, value) # 等价于self.name = name，因此触发递归


def check_type(cls):
    sig = inspect.signature(cls)
    for k in sig.parameters:
        if sig.parameters[k].annotation is not inspect._empty:
            setattr(cls, k, CheckType(sig.parameters[k].annotation, k))
    return cls


@check_type  # Person=check_type(Person)
class Person:
    #     name = CheckType(str, 'name')
    #     age = CheckType(int, 'age')
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

    def show(self):
        return self.name, self.age

if __name__ == "__main__":
    p1 = Person('su', 23)
    print(p1.__dict__)

    # p2 = Person('su', '23')
    # print(p2.__dict__)
```
```py?title="描述器版+类装饰器"

import inspect
from functools import wraps


class CheckType:
    def __init__(self, tpe, identifier):
        self.type = tpe
        self.identifier = identifier

    def __get__(self, instance, owner):
        return instance.__dict__[self.identifier]

    def __set__(self, instance, value):
        if not isinstance(value, self.type):
            raise TypeError(value)
        instance.__dict__[self.identifier] = value
        # setattr(instance, self.identifier, value) # 等价于self.name = name，因此触发递归


class TypeAssert:
    """doc for TypeAssert"""

    def __init__(self, cls):
        self.cls = cls
        wraps(cls)(self)

    def __call__(self, *args, **kwargs):
        sig = inspect.signature(self.cls)
        for k in sig.parameters:
            if sig.parameters[k].annotation is not inspect._empty:
                setattr(self.cls, k, CheckType(sig.parameters[k].annotation, k))
        return self.cls(*args)


@TypeAssert  # Person = TypeAssert(Person)
class Person:
    """doc for Person"""

    #     name = CheckType(str, 'name')
    #     age = CheckType(int, 'age')
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

    def show(self):
        return self.name, self.age


if __name__ == "__main__":
    p1 = Person('su', 23)
    print(p1.__dict__)
    print(Person.__doc__)

    p2 = Person('su', '23')
    print(p2.__dict__)


```
```py?title="描述器版+上下文"

import inspect
from functools import wraps


class CheckType:
    def __init__(self, tpe, identifier):
        self.type = tpe
        self.identifier = identifier

    def __get__(self, instance, owner):
        return instance.__dict__[self.identifier]

    def __set__(self, instance, value):
        if not isinstance(value, self.type):
            raise TypeError(value)
        instance.__dict__[self.identifier] = value
        # setattr(instance, self.identifier, value) # 等价于self.name = name，因此触发递归


class TypeAssert:
    """doc for TypeAssert"""

    def __init__(self, cls):
        self.cls = cls
        wraps(cls)(self)

    def __call__(self, *args, **kwargs):	# 可以将此函数体与__enter__函数体相同的代码块替换成执行__enter__方法
        # sig = inspect.signature(self.cls)
        # for k in sig.parameters:
        #     if sig.parameters[k].annotation is not inspect._empty:
        #         setattr(self.cls, k, CheckType(sig.parameters[k].annotation, k))
        self.__enter__()
        return self.cls(*args)


    # 上下文
    def __enter__(self):
        sig = inspect.signature(self.cls)
        for k in sig.parameters:
            if sig.parameters[k].annotation is not inspect._empty:
                setattr(self.cls, k, CheckType(sig.parameters[k].annotation, k))

    def __exit__(self, exc_type, exc_val, exc_tb):
        sig = inspect.signature(self.cls)
        for k in sig.parameters:
            delattr(self.cls, k)

class Person:
    """doc for Person"""

    #     name = CheckType(str, 'name')
    #     age = CheckType(int, 'age')
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

    def show(self):
        return self.name, self.age


if __name__ == "__main__":
    with TypeAssert(Person) as f:
        p1 = Person('su', 23)
        print(p1.__dict__)
        print(Person.__doc__)


```

















#### 其他杂项













































