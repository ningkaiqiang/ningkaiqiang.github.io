---
layout: post
title: "Python中的类与对象"
subtitle: "Python Class"
date: 2020-10-25
author: "NKQ"
header-img: "img/home-bg-art.jpg"
tags:
 - Python
---

## <kbd>\_\_init\_\_</kbd>和<kbd>\_\_new\_\_</kbd>

Python中<kbd>_\_init\_\_</kbd>方法的存在作用是初始化实例对象，此方法在类的实例创建后会被自动执行以初始化对象
<kbd>\_\_new\_\_</kbd>是真正的“构造方法”，用于创建一个类的实例

看一下<kbd>_\_init\_\_</kbd>的使用

```python
class Person(object):
    def __init__(self, name, age):
        print("我是__init__")
        self.name = name
        self.age = age 

p = Person("ning", 22) 
print(p.name)  # 访问实例变量name
print(p.age)  # 访问实例变量age

运行结果：
我是__init__  # 实例化时自动执行__init__方法
ning
22
```

在类的实例化过程中，自动调用了<kbd>\_\_init\_\_</kbd>方法

在执行<kbd>\_\_init\_\_</kbd>之前，实例已经被创建，创建实例用到的是<kbd>\_\_new\_\_</kbd>，因此称其为真正的构造方法

看一下<kbd>\_\_new\_\_</kbd>干了什么，给<kbd>object</kbd>类中的<kbd>\_\_new\_\_</kbd>加点料

```python
class Person(object):
    def __new__(cls, *args, **kwargs):
        print("我是__new__")
        instance = super(Person,cls).__new__(cls)
        return instance

    def __init__(self, name, age):
        print("我是__init__")
        self.name = name
        self.age = age

p = Person("ning", 22)
print(p.name)  # 访问实例变量name
print(p.age)  # 访问实例变量age

运行结果：
我是__new__  # 创建对象需要执行__new__方法
我是__init__  # 实例化时自动执行__init__方法
ning
22
```

<kbd>\_\_new\_\_</kbd>可以让我们控制一些类的实例化过程与结果，我们可以通过它来实现一些特有功能或者使用关于元类的一些东西

```python
class myint(int):
    def __new__(cls, *args,**kwargs):
        print('我是 __new__')
        return super(myint,cls).__new__(cls,abs(*args))  # abs方法用于取绝对值

print(myint(-1))
print(int(-1))

运行结果：
我是__new__
1
-1
```

<kbd>\_\_new\_\_</kbd>可以让我们实现单一实例（生成的每个实例都是同一个实例）

```python
class Single(object):
    def __new__(cls):
        if not hasattr(cls, 'myinstance'):  # hasattr() 函数用于判断对象是否包含对应的属性
            cls.myinstance = super(Single, cls).__new__(cls)
        return cls.myinstance  # 每次生成的都是同一个实例


obj1 = Single()
obj2 = Single()

obj1.attr1 = 'wd'
print(obj1.attr1, obj2.attr1)
print(obj1 is obj2)  # 返回True表明是同一个实例

运行结果：
wd wd
True
```

## 父类与子类<kbd>\_\_init\_\_</kbd>的使用

当一个子类被创建时（不是实例化）可以不写<kbd>\_\_init\_\_</kbd>方法，实例化它时会隐式使用父类的<kbd>\_\_init\_\_</kbd>方法，但如果子类定义了一个<kbd>\_\_init\_\_</kbd>方法，那么在子类实例化时不会隐式使用父类的<kbd>\_\_init\_\_</kbd>方法，此时如果仍想使用父类的<kbd>\_\_init\_\_</kbd>方法，需要在子类中显式调用父类<kbd>\_\_init\_\_</kbd>方法

## Python 继承、多态

Python 可以继承父类中的方法与属性并进行方法重写以实现新的子类独有功能

## Python 的多继承 Mixln

在设计类的继承时，主线一般是单一继承下来，但是如果需要额外的功能，在 Python 中可以通过多继承实现，Python 支持同时继承多个父类，这种设计被称为 Mixln

这样获取更多的扩展功能时，不需要通过复杂庞大的继承链条，直接使用多继承即可

## Python 的 object 类

| python 2.x | python 2.x       | python 3.x       | python 3.x       |
| ---------- | ---------------- | ---------------- | ---------------- |
| 不继承object | 继承object       | 不继承object也会自动继承object | 继承object       |
| \_\_doc\_\_    | \_\_doc\_\_  | \_\_doc\_\_      | \_\_doc\_\_      |
| \_\_module\_\_ | \_\_module\_\_ | \_\_module\_\_  | \_\_module\_\_  |
| 自定义属性 | 自定义属性   | 自定义属性   | 自定义属性   |
|            | \_\_class\_\_    | \_\_class\_\_    | \_\_class\_\_    |
|            | \_\_delattr\_\_  | \_\_delattr\_\_  | \_\_delattr\_\_  |
|            | \_\_dict\_\_     | \_\_dict\_\_     | \_\_dict\_\_     |
|            | \_\_format\_\_   | \_\_format\_\_   | \_\_format\_\_   |
|   |\_\_getattribute\_\_ | \_\_getattribute\_\_ | \_\_getattribute\_\_ |
|            | \_\_hash\_\_     | \_\_hash\_\_     | \_\_hash\_\_     |
|            | \_\_init\_\_     | \_\_init\_\_     | \_\_init\_\_     |
|            | \_\_new\_\_      | \_\_new\_\_      | \_\_new\_\_      |
|            | \_\_reduce\_\_   | \_\_reduce\_\_   | \_\_reduce\_\_   |
|           | \_\_reduce_ex\_\_| \_\_reduce_ex\_\_ | \_\_reduce_ex\_\_ |
|            | \_\_repr\_\_     | \_\_repr\_\_     | \_\_repr\_\_     |
|            | \_\_setattr\_\_  | \_\_setattr\_\_  | \_\_setattr\_\_  |
|            | \_\_sizeof\_\_   | \_\_sizeof\_\_   | \_\_sizeof\_\_   |
|            | \_\_str\_\_      | \_\_str\_\_      | \_\_str\_\_      |
|   | \_\_subclasshook\_\_ | \_\_subclasshook\_\_ | \_\_subclasshook\_\_|
|            | \_\_weakref\_\_  | \_\_weakref\_\_  | \_\_weakref\_\_  |
|            |                  | \_\_dir\_\_      | \_\_dir\_\_      |
|            |                  | \_\_eq\_\_       | \_\_eq\_\_       |
|            |                  | \_\_ge\_\_       | \_\_ge\_\_       |
|            |                  | \_\_gt\_\_       | \_\_gt\_\_       |
|            |                  | \_\_le\_\_       | \_\_le\_\_       |
|            |                  | \_\_lt\_\_       | \_\_lt\_\_       |
|            |                  | \_\_ne\_\_       | \_\_ne\_\_       |

## <kbd>@property</kbd>

在我们使用类时，需要对一些属性进行限制，不允许其随意改变。此时可以用封装的思想设置一个<kbd>get</kbd>方法和一个<kbd>set</kbd>方法，在访问此属性时需要调用方法，这样代码就显得很啰嗦复杂，这时候就需要用到装饰器

通过<kbd>@property</kbd>可以使得<kbd>get</kbd>方法（将<kbd>get</kbd>写成一个与属性同名的方法即可）变成一个属性，既保留了封装的优点又可以像实例属性一样直接访问属性

只设置一个<kbd>get</kbd>方法可以将属性设置为只读属性

放一个讲解[链接🔗](https://www.liaoxuefeng.com/wiki/1016959663602400/1017502538658208)

## <kbd>\_\_slots\_\_</kbd>

Python 是一门动态语言，可以在运行过程中添加属性与方法，十分灵活

```python
class Student(object):
    pass

s = Student()
s.name = 'Michael' # 动态给实例绑定一个属性
print(s.name)

def set_age(self, age): # 定义一个函数作为实例方法
    self.age = age

from types import MethodType
s.set_age = MethodType(set_age, s) # 给实例绑定一个方法，仅此实例可以使用
s.set_age(25) # 调用实例方法
print(s.age) # 测试结果

s2 = Student() # 创建新的实例
# s2.set_age(25) # 尝试调用方法，报错 ---> Student对象不存在set_age方法属性

def set_score(self, score):
    self.score = score
Student.set_score = set_score  # 给类绑定一个方法，所有实例均可使用

s.set_score(100)  # 绑定方法到类后，所有实例均可调用
print(s.score)
s2.set_score(99)
print(s2.score)
```

但是在运行过程中随意添加属性可能会很危险，如何加以限制呢？

可以使用<kbd>\_\_slots\_\_</kbd>变量来控制一个类可以添加的属性，如何使用？

```python
__slots__ = ('name', 'age')  # 定义类时使用元组来指定可添加的属性，未出现在元组中的属性被添加时会报错 AttributeError
```

注意：子类无法直接继承父类的<kbd>\_\_slots\_\_</kbd>变量，一般，<kbd>\_\_slots\_\_</kbd>变量仅在当前类中生效，但是当子类中也定义了一个<kbd>\_\_slots\_\_</kbd>变量，那么子类中可以添加的变量为子类<kbd>\_\_slots\_\_</kbd>变量加上父类的<kbd>\_\_slots\_\_</kbd>变量

## <kbd>\_\_call\_\_</kbd>
在类中定义一个<kbd>\_\_call\_\_</kbd>方法可以实现对实例的直接调用

对象是否可调用如何判断？请使用<kbd>callable()</kbd>方法

## 元类

元类的概念好像很复杂

其实只需要一句话——元类就是类(class)的类型，读三遍就能加深理解✅😅

## <kbd>type()</kbd>

我们知道，Python中可以使用<kbd>type()</kbd>来查看一个对象的类型

```python
print(type('new'),type(1),type([]),type(()))

运行结果：
<class 'str'> <class 'int'> <class 'list'> <class 'tuple'>
```

我们还知道，Python中一切都是对象，那么一个类对象的类型是什么？注意，这里说的不是类的实例化，仅仅是用一个<kbd>class</kbd>定义的类对象

```python
class Test(object):
    def __init__(self):
        super().__init__()
    def test(self):
        pass

print(type(Test()))

运行结果：
<class '__main__.test'>
```

再看一下一些内置类的类型

```python
print(type(int),type(str))

运行结果：
<class 'type'> <class 'type'>
```

哦？这些类的类型是<kbd>type</kbd>? 那么<kbd>type</kbd>的类型是什么，继续看

```python
print(type(type))

运行结果：
<class 'type'>
```

看来到这里就结束了，<kbd>type</kbd>的类型还是<kbd>type</kbd>

<kbd>type</kbd>就是Python中内置的元类，我们自己所定义的类可以实例化，生成一个对象，元类也可以实例化，生成一个类(由我们定义的)。

```
元类 ---> 类 ---> 实例
```

在Python中，可以使用<kbd>type</kbd>来创建一个类，试一下

```python
def testadd(self,a,b):
    return a+b

Test = type('Test', (object,), dict(testadd=testadd))

t = Test()
print(t.testadd(1,1))
print(type(Test))

运行结果：
2
<class 'type'>
```

通过<kbd>type</kbd>和用<kbd>class</kbd>定义的类是一样的东西，都是‘普通的类’，Python是一门动态语言，当解释器遇到<kbd>class</kbd>关键字时，原来只是识别一下语法，然后再用<kbd>type</kbd>创建一个类

这就是元类的作用，按我们的标准生产类

## 类的创建过程

这一部分是搬运[链接🔗](https://lotabout.me/2018/Understanding-Python-MetaClass/)

![类的创建](img/in-post/../../../img/in-post/Python-class/IMAGE%202020-10-25%2017:19:04.jpg)

1. 当 Python 见到<kbd>class</kbd>关键字时，会首先解析<kbd>class...</kbd>中的内容。例如解析基类信息，最重要的是找到对应的元类信息（默认是 <kbd>type</kbd>)
2. 元类找到后，Python 需要准备 namespace ，如果元类实现了 \_\_prepare\_\_ 函数，则会调用它来得到默认的namespace 
3. 之后是调用 <kbd>exec</kbd>来执行类的 body，包括属性和方法的定义，最后这些定义会被保存进 namespace
4. 上述步骤结束后，就得到了创建类需要的所有信息，这时 Python 会调用元类的构造函数来真正创建类

如果你想在类的创建过程中做一些定制(customization)的话，创建过程中任何用到了元类的地方，我们都能通过覆盖元类的默认方法来实现定制。这也是元类“无所不能”的所在，它深深地嵌入了类的创建过程

## 元类的应用

> Metaclasses are deeper magic that 99% of users should never worry about it. If you wonder whether you need them, you don't (the people who actually need them to know with certainty that they need them and don't need an explanation about why).
>
>*Python Guru Tim Peters*

等我能用到再写吧！

建议阅读：[🔗stackoverflow](*https://stackoverflow.com/a/6581949/12523821*)的关于元类回答

