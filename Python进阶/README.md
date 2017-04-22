#Python进阶
##书籍介绍
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

##调试
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


##可迭代对象、迭代器
- **可迭代对象**：Python中任意的对象，只要它定义了可以返回一个迭代器的`__iter__`方法，或者定义了可以支持下标索引的`__getitem__`方法(这些双下划线方法会在其他章节中全面解释)，那么它就是一个可迭代对象。
- **迭代器**：任意对象，只要定义了`next`(Python2) 或者`__next__`方法，它就是一个迭代器。

`for`循环会自动捕捉`StopIteration`异常并停止调用`next()`。 

Python的一些内置对象也支持迭代：
```python
my_string = "test"
my_iter = iter(my_string)
next(my_iter)
```

##map, filter, reduce
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

##set
检查列表中是否包含重复的元素
```python
some_list = ['a', 'b', 'c', 'b', 'd', 'm', 'n', 'n']
duplicates = set([x for x in some_list if some_list.count(x) > 1])
print(duplicates)
```

##三元运算符
```python
is_fat = True
state = "fat" if is_fat else "not fat"
```

##变量&对象
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

##装饰器
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


