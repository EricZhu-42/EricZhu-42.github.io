---
title: Functional Programming of Python
date: 2019-03-26
tags: [python]
---

以下介绍Python中**函数式编程**的一些基础知识。

#### 高阶函数 (Higher-order Function)

1. 什么是高阶函数？

   **一切**的Python对象都是**第一类对象**，这意味着可以将一切可命名的对象直接当作数据进行处理（如函数，模块，异常等）。因此，变量可以指向一个函数，一切可以由变量占据的位置也可以由函数替代。

   **高阶函数**指接受一个函数作为参数的函数。以下为一个简单的例子。

    ```python
    def add(x, y, f):
        return f(x) + f(y)
    ```

2. map() & reduce()

   <!-- more --> 

   > `map`(*function*, *iterable*, *...*)  
   > 	Return an iterator that applies *function* to every item of *iterable*, yielding the results. If additional *iterable*arguments are passed, *function* must take that many arguments and is applied to the items from all iterables in parallel. With multiple iterables, the iterator stops when the shortest iterable is exhausted.

   `map()`函数接收两个参数，一个是函数，一个是`Iterable`，`map`将传入的函数依次作用到序列的每个元素，并把结果作为新的`Iterator`返回。

   `reduce`把一个函数作用在一个序列`[x1, x2, x3, ...]`上，这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算。其效果近似等价于：

   ```python
    reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
   ```

   在使用map()和reduce()时，常常使用lambda函数来简化不必要的函数定义。

3. filter（）

    `filter()`也接收一个函数和一个序列。和`map()`不同的是，`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素。

    例如，把一个序列中的空字符串删掉，可以这么写：

    ```python
    def not_empty(s):
        return s and s.strip()
    
    list(filter(not_empty, ['A', '', 'B', None, 'C', '  ']))
    # Result: ['A', 'B', 'C']
    ```

    注意到`filter()`函数返回的是一个`Iterator`，也就是一个惰性序列，所以要强迫`filter()`完成计算结果，需要用`list()`函数获得所有结果并返回list。

4. sorted()

    `sorted()`函数也是一个高阶函数，它还可以接收一个`key`函数来实现自定义的排序。key指定的函数将作用于list的每一个元素上，并根据key函数返回的结果进行排序。

    要进行反向排序，不必改动key函数，可以传入第三个参数`reverse=True`。

#### 函数作为返回值

高阶函数除了可以接受函数作为参数外，还可以把函数作为结果值返回。

在求和问题时，若求和时不需要得到结果，则可以不返回求和的结果，而是返回求和的函数：

```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum

>>> f = lazy_sum(1, 3, 5, 7, 9)
>>> f
<function lazy_sum.<locals>.sum at 0x101c6ed90>

>>> f()
25
```

在这个例子中，我们在函数`lazy_sum`中又定义了函数`sum`，并且，内部函数`sum`可以引用外部函数`lazy_sum`的参数和局部变量，当`lazy_sum`返回函数`sum`时，相关参数和变量都保存在返回的函数中。这种程序结构称为**闭包**。

> 返回闭包时牢记一点：返回函数不要引用任何循环变量，或者后续会发生变化的变量。

#### 匿名函数

可以通过lambda表达式定义匿名函数。

```python
>>> list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

匿名函数有个限制，就是只能有一个表达式，不用写`return`，返回值就是该表达式的结果。匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数。同样，也可以把匿名函数作为返回值返回。

#### 装饰器

装饰器 (decorator) 是一个返回函数的高阶函数。使用装饰器时，借助Python的@语法，把decorator置于函数的定义处。

```python
def log(func):
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper

@log
def now():
    print('2015-3-25')

>>> now()
call now():
2015-3-25
```

把`@log`放到`now()`函数的定义处，相当于执行了语句：

```python
now = log(now)
```

`wrapper()`函数的参数定义是`(*args, **kw)`，因此，`wrapper()`函数可以接受任意参数的调用。如果decorator本身需要传入参数，那就需要编写一个返回decorator的高阶函数，写出来会更复杂。

另外，被`wrapper()`装饰后的函数签名会变为`wrapper`，这会导致一些依赖函数签名的应用出错。为了解决这个问题，可以用python内置的 `functools.wraps` 装饰`wrapper()`. 因此，一个完整的decorator的写法如下：

```python
import functools

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
```

或针对带参数的decorator：

```python
import functools

def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
```

#### 偏函数

**偏函数**（`functools.partial`）的作用是把一个函数的某些参数固定住（也就是设置默认值），返回一个新的函数。这使得调用这个新函数会更加简单。

```python
>>> import functools
>>> int2 = functools.partial(int, base=2)
>>> int2('1000000')
64
>>> int2('1010101')
85
>>> int2('1000000', base=10)
1000000
```

参考资料：

1. [函数式编程](<https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014317848428125ae6aa24068b4c50a7e71501ab275d52000>)
2. [Python 3.7.3 documentation](<https://docs.python.org/3/library/functions.html>)