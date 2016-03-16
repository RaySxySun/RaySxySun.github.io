---
layout: post
title: 1. Have a deeper insight into Phython class and metaclass
date: 2016-01-06
categories: python
tags: [Python]
description: To understand python class and metaclass
---
> Reference: [Stack overflow][1] & [The blog][2]


## 1\. 类也是对象
------
理解metaclass之前要先理解Python Class. 和其他类一样， 我们可以声明一个实例。
    
    class ObjectCreator(object):
        pass
   
    my_object = ObjectCreator()
    print my_object
    <__main__.ObjectCreator object at 0x8974f2c>


另外，这个class本身也是实例

    class ObjectCreator(object):
        pass
   

因此即使不声明这个类， 我们也可以对它做如下操作：

    print ObjectCreator     
    # 你可以打印一个类，因为它其实也是一个对象
    <class '__main__.ObjectCreator'>
    def echo(o):
        print o
   
    echo(ObjectCreator)                 
    # 你可以将类做为参数传给函数
    <class '__main__.ObjectCreator'>
    print hasattr(ObjectCreator, 'new_attribute')
    > Fasle
    ObjectCreator.new_attribute = 'foo' 
    #  你可以为类增加属性
    print hasattr(ObjectCreator, 'new_attribute')
    > True
    print ObjectCreator.new_attribute
    >foo
    ObjectCreatorMirror = ObjectCreator 
    # 你可以将类赋值给一个变量
    print ObjectCreatorMirror()
    <__main__.ObjectCreator object at 0x8997b4c>

1.  你可以将它赋值给一个变量
2.  你可以拷贝它
3.  你可以为它增加属性
4.  你可以将它作为函数参数进行传递

    
    
## 2\. 动态地创建类
------
类也是对象， 我们可以在运行是创建，像其他类一样。

    def choose_class(name):
         if name == 'foo':
             class Foo(object):
                 pass
             return Foo     
    # 返回的是类，不是类的实例
         else:
             class Bar(object):
                 pass
             return Bar
    
    MyClass = choose_class('foo')
    print MyClass              
    # 函数返回的是类，不是类的实例
    <class '__main__'.Foo>
    print MyClass()            
    # 你可以通过这个类创建类实例，也就是对象
    <__main__.Foo object at 0x89c6d4c>
    
当你使用class关键字时，Python解释器自动创建这个对象。Python仍然提供给你手动处理的方法。函数type能够让我们知道一个对象的类型是什么。

    print type(1)
    <type 'int'>
    print type("1")
    <type 'str'>
    print type(ObjectCreator)
    <type 'type'>
    print type(ObjectCreator())
    <class '__main__.ObjectCreator'>
值得注意的是type有一种完全不同的能力，它也能动态的创建类。
（同一个函数拥有两种完全不同的用法是一件很傻的事情，但这在Python中是为了保持向后兼容性））
    
    type(类名, 父类的元组（针对继承的情况，可以为空），包含属性的字典（名称和值）)

比如下面的代码：
    
    class MyShinyClass(object):
        pass
        
也可以手动创建

    MyShinyClass = type('MyShinyClass', (), {})  
    # 返回一个类对象
    print MyShinyClass
    <class '__main__.MyShinyClass'>
    print MyShinyClass()  
    #  创建一个该类的实例
    <__main__.MyShinyClass object at 0x8997cec>
    
“MyShinyClass”当做一个变量来作为类的引用。类和变量是不同的。
ype 接受一个字典来为类定义属性。

    class Foo(object):
        bar = True
    
等价于

    Foo = type('Foo', (), {'bar':True})
  
Foo如同普通类

    print Foo
    <class '__main__.Foo'>
    print Foo.bar
    > True
    f = Foo()
    print f
    <__main__.Foo object at 0x8a9b84c>
    print f.bar
    > True  
    
也可继承该类

    class FooChild(Foo):
        pass 

可以写成：    

    FooChild = type('FooChild', (Foo,),{})
    print FooChild
    <class '__main__.FooChild'>
    print FooChild.bar   
    # bar属性是由Foo继承而来
    > True
    
最后我们会希望为子类增加方法签名
    def echo_bar(self):
        print self.bar
    
    FooChild = type('FooChild', (Foo,), {'echo_bar': echo_bar})
    hasattr(Foo, 'echo_bar')
    > False
    hasattr(FooChild, 'echo_bar')
    > True
    my_foo = FooChild()
    my_foo.echo_bar()
    > True

**在Python中，类也是对象**
动态的创建类是当你使用关键字class时Python在幕后做的事情.
这就是通过元类来实现的。

## 3 \. 什么是元类    
------
MetaClass就是用来创建类的“类”
    
    MyClass = MetaClass()
    MyObject = MyClass()
    
等价与type函数
函数type实际上是一个元类。
type就是Python在用来创建所有类的元类。
type会全部采用小写形式而不是Type

    MyClass = type('MyClass', (), {})

str是用来创建字符串对象的类，int是用来创建整数对象的类，type就是创建类对象的类
通过检查__class__属性来看来观察

    age = 35
    age.__class__
    <type 'int'>
    name = 'bob'
    name.__class__
    <type 'str'>
    def foo(): pass
    foo.__class__
    <type 'function'>
    class Bar(object): pass
    b = Bar()
    b.__class__
    <class '__main__.Bar'>
    
任何一个__class__的__class__属性

    a.__class__.__class__
    <type 'type'>
    age.__class__.__class__
    <type 'type'>
    foo.__class__.__class__
    <type 'type'>
    b.__class__.__class__
    <type 'type'>

元类可称为“类工厂”(不是工厂类)

**__metaclass__属性**

可在写一个类的时候为其添加__metaclass__属性。

    class Foo(object):
        __metaclass__ = something…
    […]

流程： Foo中有__metaclass__这个属性吗？

是： Python会在内存中通过__metaclass__创建一个名字为Foo的类对象。

否： 继续在父类中寻找__metaclass__属性 -> 父类中都找不到 -> 会在模块层次中去寻找 -> 还是找不到 -> 用内置的type来创建这个类对象。

***__metaclass__中放置些什么?type, 任何使用到type, 子类化type***

**自定义元类**

我们决定在模块里所有的类的属性都应该是大写形式。其中一种方法就是通过在模块级别设定__metaclass__。

1.用方法来定义__metaclass__ 

    # 元类会自动将你通常传给‘type’的参数作为自己的参数传入
    def upper_attr(future_class_name, future_class_parents, future_class_attr):
        
    '''返回一个类对象，将属性都转为大写形式'''
        
    #  选择所有不以'__'开头的属性
        attrs = ((name, value) for name, value in future_class_attr.items() if not name.startswith('__'))
        # 将它们转为大写形式
        uppercase_attr = dict((name.upper(), value) for name, value in attrs)
        # 通过'type'来做类对象的创建
        return type(future_class_name, future_class_parents, uppercase_attr)
    __metaclass__ = upper_attr  
    #  这会作用到这个模块中的所有类
    class Foo(object):
    
    # 我们也可以只在这里定义__metaclass__，这样就只会作用于这个类中
    bar = 'bip'
    print hasattr(Foo, 'bar')
    # 输出: False
    print hasattr(Foo, 'BAR')
    # 输出:True
     
    f = Foo()
    print f.BAR
    # 输出:'bip'

2.用类来定义__metaclass__

    # 请记住，'type'实际上是一个类，就像'str'和'int'一样
    # 所以，你可以从type继承
    class UpperAttrMetaClass(type):
        
    # __new__ 是在__init__之前被调用的特殊方法
        
    # __new__是用来创建对象并返回之的方法
        
    # 而__init__只是用来将传入的参数初始化给对象
        
    # 你很少用到__new__，除非你希望能够控制对象的创建
        
    # 这里，创建的对象是类，我们希望能够自定义它，所以我们这里改写__new__
        
    # 如果你希望的话，你也可以在__init__中做些事情
        
    # 还有一些高级的用法会涉及到改写__call__特殊方法，但是我们这里不用
        def __new__(upperattr_metaclass, future_class_name, future_class_parents, future_class_attr):
            attrs = ((name, value) for name, value in future_class_attr.items() if not name.startswith('__'))
            uppercase_attr = dict((name.upper(), value) for name, value in attrs)
            return type(future_class_name, future_class_parents, uppercase_attr)
            
**这种方式其实不是OOP**
调用了type,没有改写父类的__new__。现在我们这样去处理:
    
    class UpperAttrMetaclass(type):
        def __new__(upperattr_metaclass, future_class_name, future_class_parents, future_class_attr):
            attrs = ((name, value) for name, value in future_class_attr.items() if not name.startswith('__'))
            uppercase_attr = dict((name.upper(), value) for name, value in attrs)
     
            
    # 复用type.__new__方法
            
    # 这就是基本的OOP编程
            return type.__new__(upperattr_metaclass, future_class_name, future_class_parents, uppercase_attr)
    
                
相比1 多一个参数upperattr_metaclass， 本质是自定义__metaclass__类的self。真实的产品代码中这个元类参数名字是这样：

    class UpperAttrMetaclass(type):
        def __new__(cls, name, bases, dct):
            attrs = ((name, value) for name, value in dct.items() if not name.startswith('__')
            uppercase_attr  = dict((name.upper(), value) for name, value in attrs)
            return type.__new__(cls, name, bases, uppercase_attr)
            
就是这样，元类可以成为“黑暗魔法”， 会让写出更复杂的程序。

**元类本身是很简单：**

1.  拦截类的创建
2.  修改类
3.  返回修改之后的类

**为什么要用metaclass类而不是函数**

1）  意图会更加清晰。当你读到UpperAttrMetaclass(type)时，你知道接下来要发生什么。

2） 你可以使用OOP编程。元类可以从元类中继承而来，改写父类的方法。元类甚至还可以使用元类。

3）  你可以把代码组织的更好。当你使用元类的时候肯定不会是像我上面举的这种简单场景，通常都是针对比较复杂的问题。将多个方法归总到一个类中会很有帮助，也会使得代码更容易阅读。

4） 你可以使用__new__, __init__以及__call__这样的特殊方法。它们能帮你处理不同的任务。就算通常你可以把所有的东西都在__new__里处理掉，有些人还是觉得用__init__更舒服些。

5） 哇哦，这东西的名字是metaclass，肯定非善类，我要小心！

**为什么要使用元类？**

> "类就是深度的魔法，99%的用户应该根本不必为此操心。如果你想搞清楚究竟是否需要用到元类，那么你就不需要它。那些实际用到元类的人都非常清楚地知道他们需要做什么，而且根本不需要解释为什么要用元类。"  —— Python界的领袖 Tim Peters

元类的主要用途是创建API。Django ORM允许你像这样定义：    

    class Person(models.Model):
        name = models.CharField(max_length=30)
        age = models.IntegerField()
    guy  = Person(name='bob', age='35')
    print guy.age
    
print guy.age 并不一定会返回一个IntegerField对象， 一些暗黑魔法能够将你刚刚定义的简单的Person类转变成对数据库的一个复杂**hook**。 

## 4\. 结语
类能创建出类实例的对象, 类本身也是实例,它们是元类的实例。 

    class Foo(object): pass
    id(Foo)
    >142630324  
    
1. Python中的一切都是对象，它们要么是类的实例，要么是元类的实例，除了type。type是自己的元类，Python环境中我们自己做不到。
2. 元类是很复杂的。对于非常简单的类，你可能不希望通过使用元类来对类做修改。**动态修改类**两种技术来修改类(99%)：

> * Monkey patching * Class decorators  
    

[1]: http://stackoverflow.com/questions/100003/what-is-a-metaclass-in-python
[2]: http://blog.jobbole.com/21351/