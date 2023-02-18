## Python装饰器解析

---

**python中的函数可以像普通变量一样当作参数传递给另外一个函数**

```python
def foo():
	print("foo is starting")
	
def bar(func):
	func()
	
if __name__ == "__main__":
	bar(foo)
	# print foo is starting
```

装饰器的本质上是一个python函数或者类，它可以让其他函数或者类在不需要做任何代码修改的前提下增加额外的功能，装饰器的返回值也是一个函数或者类对象。

装饰器的常见用处：

	* 插入日志
	* 性能测试
	* 权限校验

---

### 简单的装饰器

```python
def use_log(func):
	def wrapper():
		logging.info("%s is running"% func.__name__)
		return func()
	return wrapper
	
def foo():
	print("I am foo")
    
    
if __name__ == "__main__":
    # 因为装饰器use_log(foo)返回时函数对象wrapper，这条语句相当于foo=warpper
    foo = use_log(foo)
     # 执行foo()就相当于执行 wrapper()
    foo()
```

### @语法糖

 @ 符号就是装饰器的语法糖，它放在函数开始定义的地方，这样就可以省略最后一步再次赋值的操作。

```python
def use_log(func):
	def wrapper():
		logging.info("%s is running"% func.__name__)
		return func()
	return wrapper
	
@use_log
def foo():
	print("I am foo")
    
    
if __name__ == "__main__":
    foo()
```

如上所示，有了 @ ，我们就可以省去`foo = use_logging(foo)`这一句了，直接调用 foo() 即可得到想要的结果。

---

### *args **kwargs

```python
def wrapper(*args, **kwargs):
    logging.warn("%s is running" % func.__name__)
        return func(*args, **kwargs)
    return wrapper
```

### 带参数的装饰器

​		装饰器还有更大的灵活性，例如带参数的装饰器，在上面的装饰器调用中，该装饰器接收唯一的参数就是执行业务的函数 foo 。装饰器的语法允许我们在调用时，提供其它参数，比如`@decorator(a)`。这样，就为装饰器的编写和使用提供了更大的灵活性。比如，我们可以在装饰器中指定日志的等级，因为不同业务函数可能需要的日志级别是不一样的。

```python
def use_log(level):
	def decorator(func):
        def wrapper(*args, **kwargs):
            if level == "warn":
                logging.warn("%s is running" % func.__name__)
             elif level == "info":
                logging.info("%s is running" % func.__name__)
             return func(*args)
        return wrapper
    return decorator

@use_log(level="info")
def foo(name="foo"):
       print("I am %s" % name)
        
foo()
```

### 类装饰器

​	没错，装饰器不仅可以是函数，还可以是类，相比函数装饰器，类装饰器具有灵活度大、高内聚、封装性等优点。使用类装饰器主要依靠类的`__call__`方法，当使用 @ 形式将装饰器附加到函数上时，就会调用此方法。

```python
class Foo(object):
    def __init__(self, func):
        self._func = func

    def __call__(self):
        print ('class decorator runing')
        self._func()
        print ('class decorator ending')

@Foo
def bar():
    print ('bar')

bar()
```

### functools.wraps

使用装饰器极大地复用了代码，但是他有一个缺点就是原函数的元信息不见了，比如函数的`docstring`、`__name__`、参数列表，先看例子

```python
# 装饰器
def logged(func):
    def with_logging(*args, **kwargs):
        print func.__name__      # 输出 'with_logging'
        print func.__doc__       # 输出 None
        return func(*args, **kwargs)
    return with_logging

# 函数
@logged
def f(x):
   """does some math"""
   return x + x * x

logged(f)
```

不难发现，函数 f 被`with_logging`取代了，当然它的`docstring`，`__name__`就是变成了`with_logging`函数的信息了。好在我们有`functools.wraps`，`wraps`本身也是一个装饰器，它能把原函数的元信息拷贝到装饰器里面的 func 函数中，这使得装饰器里面的 func 函数也有和原函数 foo 一样的元信息了。

```python
from functools import wraps
def logged(func):
    @wraps(func)
    def with_logging(*args, **kwargs):
        print func.__name__      # 输出 'f'
        print func.__doc__       # 输出 'does some math'
        return func(*args, **kwargs)
    return with_logging

@logged
def f(x):
   """does some math"""
   return x + x * x
```



### 经典面试题

```python
# 有一个函数bar()，偶尔会抛出超时异常（TimeoutError)
# 请完成retry，使bar()在遇到这种异常时可以重试指定的次数 
def retry(exception: Exception, max_retries: int):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for i in range(max_retries):
                try:
                    return func(*args, **kwargs)
                except TimeoutError:
                    print(f"第{i+1}次重试")
                return None
        return wrapper
    return decorator

class TimeoutError(Exception):
    pass

@retry(TimeoutError, 10)
def bar(*args, **kwargs):
    import random
    if random.random() < 0.5:
        raise TimeoutError("bar failed")
```

