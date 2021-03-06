# Python语言特性

## 1. python 异步发展历史
1. python 中首先有了yield

    此时yield仅能够 作为表达式存在，此时仅能够作为generator存在
```python
def foo():
    a = 0
    while True:
        yield a
        if a == 10:
            break
```

2. PEP 342 — Coroutines via Enhanced Generators

    引入了send， throw， close 方法
此时 yield 可以作为表达式 存在于赋值号（=）的右边
这时候为了避免混淆，应该把下面这段代码称为 coroutine，而不是generator
```python
def simple_coroutine():
    print('-> coroutine started')
    x = yield
    print('-> coroutine received:', x)
```

3. PEP 380 -- Syntax for Delegating to a Subgenerator
    
    yield from 被提出
    - yield from 被用做 generator 的嵌套使用
    - yield from 被用做 coroutine 的嵌套使用

4. PEP 492 — Coroutines with async and await syntax
此时 corotine 正式的独立于 generator 存在，
generator 回归 到 yield 作为 单独存在
yield 作为复制号（=）右边的情况 & yield from 关键字 被用做coroutine的嵌套的情况 应该处于 deprecated 状态。
yield 应该仅作为 generator 使用，yield from 应该仅作为 generator 嵌套使用。


更多深入理解参照：

[深入理解Python异步编程](https://github.com/denglj/aiotutorial)

[理解Python asyncio 内部实现机制](https://lotabout.me/2017/understand-python-asyncio/?utm_source=tuicool&utm_medium=referral)

[js的单线程和异步](https://www.cnblogs.com/woodyblog/p/6061671.html)


 



## 2 理解Iterable、Iterator、Generator
题目：使用 \_\_iter__ & \_\_next__ 实现类似 for i in range(10) 这样的功能

答案：使用Iterable 和 Iterator
```python
class RangerIterable:
    def __init__(self, end):
        self.end = end
    def __iter__(self):
        return RangerIterator(self.end)

class RangerIterator:
    def __init__(self, end):
        self.value = -1
        self.end = end
    def __iter__(self):
        return self
    def __next__(self):
        while True:
            self.value += 1
            if self.value == end:
                raise StopIteration
            return self.value

for i in RangerIterable(10):
    print(i)
```
答案：generator 版本，语言更加精炼
```python
class RangerIterable:
    def __init__(self, end):
        self.value = -1
        self.end = end
    def __iter__(self):
        while True:
            self.value += 1
            if self.value == end:
                break
            yield self.value
for i in RangerIterable(10):
    print(i)
```

## 3 __dict__ & dir() 区别

dir()函数会自动寻找一个对象的所有属性，包括__dict__中的属性。

__dict__是dir()的子集，dir()包含__dict__中的属性。

详细参见：
https://www.cnblogs.com/YingxuanZHANG/p/8805836.html

参见：
https://docs.python.org/3/library/functions.html中

builtin functions 

vars()

dir()




## 4. Python垃圾回收机制

详细情况参见：

[Python 中的垃圾回收机制](http://python.jobbole.com/87843/)

[Python垃圾回收机制--完美讲解!](https://www.cnblogs.com/pinganzi/p/6646742.html)

 

Python GC主要使用引用计数（reference counting）来跟踪和回收垃圾。在引用计数的基础上，通过“标记-清除”（mark and sweep）解决容器对象可能产生的循环引用问题，通过“分代回收”（generation collection）以空间换时间的方法提高垃圾回收效率。

### 4.1. 引用计数

PyObject是每个对象必有的内容，其中`ob_refcnt`就是做为引用计数。当一个对象有新的引用时，它的`ob_refcnt`就会增加，当引用它的对象被删除，它的`ob_refcnt`就会减少.引用计数为0时，该对象生命就结束了。

优点:

1. 简单
2. 实时性

缺点:

1. 维护引用计数消耗资源
2. 循环引用

### 4.2. 标记-清除机制

基本思路是先按需分配，等到没有空闲内存的时候从寄存器和程序栈上的引用出发，遍历以对象为节点、以引用为边构成的图，把所有可以访问到的对象打上标记，然后清扫一遍内存空间，把所有没标记的对象释放。

### 4.3 分代技术


## 5. 闭包经典例子
```python
funcs = [lambda: i for i in range(10)]
funcs[0]()
```

[什么是闭包？](https://www.zhihu.com/question/34210214)

链接是个知乎问答，从各个角度分析分析了闭包，

其中我最喜欢下面这两个回答
- 我叫独孤求败，我在一个山洞里。。。

这个阐释的角度是对于闭包函数自己来说，它自己也不知道这个变量从哪来的，它只是能够保有这个这个变量的引用。

对于外界函数来说，它申明的变量被一个函数闭包包含了，当外界函数生命结束的时候，它所申请的变量并不会被释放掉。

- 这张图，懂的人，自然懂。。。

这个阐释的角度是从数学 以及 函数式编程 combinator 的角度来阐述闭包

## 6. python2 与 python3 区别
- python2 使用 # -\*- encoding: utf-8 -*- , 
    
    python3 文件默认使用 utf-8

- python2 中字符串是 bytes，
    
    python3 中字符串默认是 codepoint

- 经典类 与 新式类

- 列表生成，临时变量不泄露 

    [i for i in range(10)]


## 7. 作用域
Python 遵循作用域  
Local -> Enclosure -> Global -> Builtin  
简称 LEGB

下面是一些有趣的例子：

```python
def foo():
    print(i)
    i = 1
foo()
# UnboundLocalError: local variable 'i' referenced before assignment
&

def bar():
    i
    print(i)
bar()
# 错误在第二行
# NameError: name 'i' is not defined
# i 没有被定义
&

i = 1
def bar():
    i
    print(i)
bar()

&
i = 1
def bar():
    print(i)
    i = 2
bar()
# UnboundLocalError: local variable 'i' referenced before assignment

&
i = 1
def bar():
    global i
    # nonlocal 用于闭包
    print(i)
    i = 2
bar()
```
