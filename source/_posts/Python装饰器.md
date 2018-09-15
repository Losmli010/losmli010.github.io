---
title: Python装饰器
date: 2018-07-08 23:13:10
tags: [闭包,装饰器]
categories: python
---

#### 函数和变量

变量可以指向函数，函数名是一个变量。同时，函数的形参本身也是一个变量，可以接收实参，那么一个函数就可以接收另一个函数作为参数。

```python
#函数名是指向函数的变量,可以被赋值给其他变量，相当于给原函数起一个新名
new_name = abs
result = new_name(-8)		#8

#函数名作为函数的参数
map(str, [2, 4, 6, 8])
```



#### 闭包

在一个外函数中定义一个内函数，并且外函数返回内函数。一个函数和它的环境变量合在一起，就构成了一个闭包(closure) 。而环境变量是外函数的临时变量 。

```python
def outer(func):
    def inner():
        return func()
    return inner

def func():
    print("do some thing...")

f = outer(func)         
f()             #do some thing...

"""
函数被调用后才会执行
outer(func)返回inner函数, inner函数被赋值给变量f, 调用函数f()即执行inner()
"""
```



#### 闭包和装饰器

闭包可以用来书写装饰器。

```python
def outer(func):
    def inner():
        return func()
    return inner

@outer
def func():
    print("do some thing...")
    
func()           #do some thing...
```



#### 装饰器

在不修改原函数的情况下,装饰器可以在原函数调用前或调用后执行功能[装饰器就是给原函数添加功能而不用修改原函数]。

decorators are “wrappers”, which means that they let you execute code before and after the function they decorate without modifying the function itself.



##### 无参数的装饰器

```python
def hello(func):
    def wrapper():
        print("hello, %s" % func.__name__)
        func()
        print("goodbye, %s" % func.__name__)
    return wrapper

@hello
def foo():
    print("i am foo")

foo()
"""
hello, foo
i am foo
goodbye, foo
"""
#等价于foo = hello(foo)
#hello()是一个decorator，hello(foo)返回函数wrapper，调用foo()将执行新函数wrapper()
#wrapper()函数内，首先打印，再紧接着调用原始函数func(),再打印
```



##### 多个装饰器

```python
def hello(func):
    def wrapper():
        print("hello, %s" % func.__name__)
        func()
        print("goodbye, %s" % func.__name__)
    return wrapper

@hello
@hello
def bar():
    print("i am bar")

bar()
"""
hello, wrapper
hello, bar
i am bar
goodbye, bar
goodbye, wrapper
"""
#等价于bar = hello(hello(bar))
#从上向下执行,但是原函数只执行一次
#可以将hello(bar)作为一个整体func, 先给func添加功能
```



##### 有参数的装饰器

```python
def log(text):
    def decorator(func):
        def wrapper(*args, **kwargs):
            print("%s %s" % (text, func.__name__))
            func(*args, **kwargs)
        return wrapper
    return decorator

@log("hello")
def now(a):
    print("2018-5-26")
    print(a)

arg = "Learning python decorator"
now(arg)
"""
hello now
2018-5-26
Learning python decorator
"""
#等价于now = log("hello")(now)
#首先执行log("hello")，返回的是decorator函数，再调用返回的decorator函数，参数是now函数，返回值最终是wrapper函数
#wrapper可以接受不定长的参数,*args通过tuple传递参数,**kwargs通过dict传递参数
#wrapper()函数内，首先打印，再紧接着调用原始函数func()
```



##### 装饰器的修正

```python
import functools

def logger(arg):
    def decorator(func):
        @functools.wraps(func)
        def wrapper():
            print("%s %s" % (arg, func.__name__))
            return func()
        return wrapper
    return decorator

@logger("goodbye")
def today():
    print("2018-5-26")

today()
"""
goodbye today
2018-5-26
"""
print(today.__name__)
#被装饰过的原函数的函数名会变为wrapper
#@functools.wraps(func)等价于wrapper.__name__ = func.__name__
```



##### 缓存装饰器

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n <= 2:
        return 1
    return fib(n - 1) + fib(n - 2)

result = fib(10)
print(result)
print(fib.cache_info())
#CacheInfo(hits=7, misses=10, maxsize=None, currsize=10)
#被装饰的函数只执行一次，可以有效减少函数调用次数
```



##### 单例类装饰器

单例类只产生一个唯一的实例，可以用来传递值。

```python
def singleton(cls):
    instances = {}

    def wrapper(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]

    return wrapper

@singleton
class Test(object):
    def __init__(self, name):
        self.name = name
        
    def __str__(self):
        return self.name

t1 = Test("Python")			#Python
t2 = Test("C/C++")			#Python
print(id(t1) == id(t2))      #True
```



#### 参考资料

[PYTHON修饰器的函数式编程][1]

[1]: https://coolshell.cn/articles/11265.html