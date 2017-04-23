# Python进阶
## 书籍介绍
《Python进阶》是《Intermediate Python》的中文译本。  
链接：[https://www.gitbook.com/book/eastlakeside/interpy-zh/details](https://github.com/qiyidefeng/reading-notes)

## \*args **kargs使用的一个场景
猴子补丁，在程序运行的时候修改某些代码
```python
import someclass

def get_info(self, *args):
    return "Test data"

someclass.get_info = get_info
```

## 调试
- 方法一：`python -m pdb test.py`
- 方法二：脚本内部运行
```python
import pdb
pdb.set_trace()
```


- `c`: 继续执行
- `w`: 显示上下文
- `a`: 显示当前函数参数列表
- `s`: 单步进入
- `n`: 单步调过


## 可迭代对象、迭代器
- **可迭代对象**：Python中任意的对象，只要它定义了可以返回一个迭代器的`__iter__`方法，或者定义了可以支持下标索引的`__getitem__`方法(这些双下划线方法会在其他章节中全面解释)，那么它就是一个可迭代对象。
- **迭代器**：任意对象，只要定义了`next`(Python2) 或者`__next__`方法，它就是一个迭代器。

`for`循环会自动捕捉`StopIteration`异常并停止调用`next()`。 

Python的一些内置对象也支持迭代：
```python
my_string = "test"
my_iter = iter(my_string)
next(my_iter)
```

## map, filter, reduce
```python
items = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, items))
```
在python2中map直接返回列表，但在python3中返回迭代器
```python
value = map(lambda x: x(i), funcs)`
```

```python
number_list = range(-5, 5)
less_than_zero = filter(lambda x: x < 0, number_list)
```

```python
from functools import reduce
product = reduce( (lambda x, y: x * y), [1, 2, 3, 4] )
```

## set
检查列表中是否包含重复的元素
```python
some_list = ['a', 'b', 'c', 'b', 'd', 'm', 'n', 'n']
duplicates = set([x for x in some_list if some_list.count(x) > 1])
print(duplicates)
```

## 三元运算符
```python
is_fat = True
state = "fat" if is_fat else "not fat"
```

## 变量&对象
```python
def hi(name="yasoob"):
    return "hi " + name

print(hi())
# output: 'hi yasoob'

greet = hi
print(greet())
# output: 'hi yasoob'

del hi
print(hi())
#outputs: NameError

print(greet())
#outputs: 'hi yasoob'
```

## 装饰器
应用实例：授权检查
```python
from functools import wraps

def requires_auth(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        auth = request.authorization
        if not auth or not check_auth(auth.username, auth.password):
            authenticate()
        return f(*args, **kwargs)
    return decorated
```
应用实例：日志
```python
from functools import wraps

def logit(func):
    @wraps(func)
    def with_logging(*args, **kwargs):
        print(func.__name__ + " was called")
        return func(*args, **kwargs)
    return with_logging

@logit
def addition_func(x):
   """Do some math."""
   return x + x


result = addition_func(4)
# Output: addition_func was called\
```

## \_\_slots__
在Python中，每个类都有实例属性。默认情况下Python用一个字典来保存一个对象的实例属性。这非常有用，因为它允许我们在运行时去设置任意的新属性。  

然而，对于有着已知属性的小类来说，它可能是个瓶颈。这个字典浪费了很多内存。Python不能在对象创建时直接分配一个固定量的内存来保存所有的属性。因此如果你创建许多对象（我指的是成千上万个），它会消耗掉很多内存。

不过还是有一个问题。这个方法需要使用`__slots__`来告诉Python不要使用字典，而且只给一个固定集合的属性方法来规避这个分配空
```python
__slots__ = ['name', 'identifier']
```


## 容器collections
### `defaultdict`

应用实例1：不用检查key是否存在
```python
from collections import defaultdict

colours = (
    ('Yasoob', 'Yellow'),
    ('Ali', 'Blue'),
    ('Arham', 'Green'),
    ('Ali', 'Black'),
    ('Yasoob', 'Red'),
    ('Ahmed', 'Silver'),
)

favourite_colours = defaultdict(list)

for name, colour in colours:
    favourite_colours[name].append(colour)

print(favourite_colours)
```

应用实例2：嵌套赋值
```python
import collections
tree = lambda: collections.defaultdict(tree)
some_dict = tree()
some_dict['colours']['favourite'] = "yellow"
```

### `counter`
```python
favs = Counter(name for name, colour in colours)

with open('filename', 'rb') as f:
    line_count = Counter(f)
```

### deque
`deque`提供了一个双端队列


### namedtuple
可以像访问字典一样访问`namedtuples`
```python
from collections import namedtuple

Animal = namedtuple('Animal', 'name age type')
perry = Animal(name="perry", age=31, type="cat")

print(perry)

## 输出: Animal(name='perry', age=31, type='cat')

print(perry.name)

## 输出: 'perry'
```

### `enum.Enum`(python 3.4+)
from enum import Enum

class Species(Enum):
    cat = 1
    dog = 2
    kitten = 1


## 推导式
- 列表：multiples = [i for i in range(30) if i % 3 is 0]
- 字典：


```python
mcase = {'a': 10, 'b': 34, 'A': 7, 'Z': 3}

mcase_frequency = {
    k.lower(): mcase.get(k.lower(), 0) + mcase.get(k.upper(), 0)
    for k in mcase.keys()
}
```

- 集合：squared = {x**2 for x in [1, 1, 2]}


## `try/else`
else从句会在没有异常的时候执行

## 一行式
- 性能分析：`python -m cProfile my_script.py`
- 列表碾平：`print(list(itertools.chain.from_iterable(a_list)))`


## `for/else`
其中`else`语句会在`for`循环正常结束时执行


## c扩展
1. CTypes

gcc -shared -Wl,-soname,adder -o adder.so -fPIC add.c

```python
from ctypes import *
adder = CDLL('./adder.so')
res_int = adder.add_int(4,5)
```

2. python/c API
此方法使用最广泛，包含python.h头文件

## python2/3兼容
```python
from __future__ import with_statement
from __future__ import print_function
try:
    import urllib.request as urllib_request  # for Python 3
except ImportError:
    import urllib2 as urllib_request  # for Python 2
```

## 函数缓存
函数缓存允许我们将一个函数对于给定参数的返回值缓存起来。
### python3.2+
```python
from functools import lru_cache

@lru_cache(maxsize=32)
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)
```

### python2+
```python
from functools import wraps

def memoize(function):
    memo = {}
    @wraps(function)
    def wrapper(*args):
        if args in memo:
            return memo[args]
        else:
            rv = function(*args)
            memo[args] = rv
            return rv
    return wrapper
```

## 上下文管理器
`with`语句
### 基于类的实现
```python
class File(object):
    def __init__(self, file_name, method):
        self.file_obj = open(file_name, method)
    def __enter__(self):
        return self.file_obj
    def __exit__(self, type, value, traceback):
        print("Exception has been handled")
        self.file_obj.close()
        return True
```

### 基于生成器的实现
```python
from contextlib import contextmanager

@contextmanager
def open_file(name):
    f = open(name, 'w')
    yield f
    f.close()

with open_file('some_file') as f:
    f.write('hola!')

```




