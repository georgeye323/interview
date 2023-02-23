## python面试题

	### 	python基础

​	**文件操作**

​			1. 有一个jsonline格式的文件file.txt大小约为10K

```python
def getLines():
    with open('file.txt', 'rb') as f:
        return f.readlines()
if __name__ == "__main__":
    for e in getLines():
        process(e)
```

​		现在要处理一个大小为10G的文件，但是内存只有4G，如果在只修改get_lines 函数而其他代码保持不变的情况下，应该如何实现？需要考虑的问题都有那些？			

​			2. 补充缺失的代码

```python
def print_directory_contents(sPath):
    """
    这个函数接收文件夹的名称作为输入参数
    返回该文件夹中文件的路径
    以及其包含文件夹中文件的路径
    """
    import os
    for s_child in os.listdir(s_path):
        s_child_path = os.path.join(s_path, s_child)
        if os.path.isdir(s_child_path):
            print_directory_contents(s_child_path)
        else:
            print(s_child_path)
```

​	**模块与包**

​			3. 输入日期，判断这一天是这一年的第几天？

```python
import datetime
def dayofyear():
    year = input("请输入年份: ")
    month = input("请输入月份: ")
    day = input("请输入天: ")
    date1 = datetime.date(year=int(year),month=int(month),day=int(day))
    date2 = datetime.date(year=int(year),month=1,day=1)
    return (date1-date2).days+1
```

​		4. 打乱一个排好序的list对象alist?

```python
import random
alist = [1,2,3,4,5]
random.shuffle(alist)
print(alist)
```

​	**数据类型**

​		5. 现有字典 d= {'a':24,'g':52,'i':12,'k':33}请按value值进行排序?

```python
sorted(d.items(), key=lambda x:x[1])
```

​		6. 字典推导式

```python
d = {key: value for (key, value) in iterable}
```

​		7. 请反转字符串"aStr"?

```python
print(strs[::-1])
```

​		8. 将字符串 "k:1 |k1:2|k2:3|k3:4"，处理成字典 {k:1,k1:2,...}

```python
str1 = "k:1|k1:2|k2:3|k3:4"
def strToDict(str1):
    dict1 = dict()
    for items in str1.split("|"):
        key, value = items.split(":")
        dict1[key] = value
    print(dict1)


strToDict(str1)
```

​		9. 请按alist中元素的age由大到小排序

```python
alist = [{'name':'a','age':20},{'name':'b','age':30},{'name':'c','age':25}]

def listSort(lst):
    return sorted(alist, key=lambda x:x["age"], reverse=True)

result = listSort(alist)
print(result)
```

​		10. 下面代码的输出结果将是什么？

```python
list = ['a','b','c','d','e']
print(list[10:])
```

​		代码将输出[],不会产生IndexError错误，就像所期望的那样，尝试用超出成员的个数的index来获取某个列表的成员。例如，尝试获取list[10]和之后的成员，会导致IndexError。然而，尝试获取列表的切片，开始的index超过了成员个数不会产生IndexError，而是仅仅返回一个空列表。这成为特别让人恶心的疑难杂症，因为运行的时候没有错误产生，导致Bug很难被追踪到。

​		11.写一个列表生成式，产生一个公差为11的等差数列

```python  
print([x*11 for x in range(10)])
```

​		12.给定两个列表，怎么找出他们相同的元素和不同的元素？

```python 
list1 = [1,2,3]
list2 = [3,4,5]
set1 = set(list1)
set2 = set(list2)
print(set1 & set2)
print(set1 ^ set2)
```

​		13.请写出一段python代码实现删除list里面的重复元素？

```python 
l1 = ['b','c','d','c','a','a']
# 利用集合去重
l2 = list(set(l1))
print(l2)
```

​		14.给定两个list A，B ,请用找出A，B中相同与不同的元素

```python
A,B 中相同元素： print(set(A)&set(B))
A,B 中不同元素:  print(set(A)^set(B))
```

​	**企业面试题**

​		15. python新式类和经典类的区别？

   * 在python里凡是继承了object的类，都是新式类

   * python3里只有新式类

   *  Python2里面继承object的是新式类，没有写父类的是经典类

   * 经典类目前在Python里基本没有应用、

     16. python中内置的数据结构有几种？

         ```
         整型 int
         浮点型 float
         复数型 complex
         小数型 decimal
         字符串 str
         列表 list
         字典 dict
         集合 set
         元组 tuple
         ```

     17. python如何实现单例模式？请写出两种实现方式？

     ```python
     class Singleton(object):
     	
     ```

     18. python中类方法、类实例方法、静态方法有何区别？

         * 类方法：是类对象的方法，在定义时需要在上方使用”@classmethod"进行装饰，形参为cls，表示类对象，类对象和实例对象都可调用。
         * 类实例方法：是类实例化对象的方法，只有实例对象可以调用，形参为self，指对象本身
         * 静态方法：是一个任意函数，在其上方使用“@staticmethod"进行装饰，可以用对象直接调用，静态方法实际与该类没有任何关系

     19. python的内存管理机制及调优手段？

         **内存管理机制：引用计数、垃圾回收、内存池**

         **引用计数**：引用计数是一种非常高效的内存管理手段， 当一个 Python 对象被引用时其引用计数增加 1， 当其不再被一个变量引用时则计数减 1. 当引用计数等于 0 时对象被删除。

         **垃圾回收：**

         		1. 引用计数
           		2. 标记清除
           		3. 分代回收

     20. python函数调用的时候参数的传递方式是值传递还是引用传递？

         Python 的参数传递有：位置参数、默认参数、可变参数、关键字参数。函数的传值到底是值传递还是引用传递，要分情况：

         **不可变参数用值传递**

         ​	像整数和字符串这样的不可变对象，是通过拷贝进行传递的，因为你无论如何都不可能在原处改变不可变对象

         **可变参数是引用传递的**

         ​	比如像列表，字典这样的对象是通过引用传递、和 C 语言里面的用指针传递数组很相似，可变对象能在函数内部改变。

     21. map函数和reduce函数？

         (1) 从参数方面来讲：

         map()包含两个参数，第一个参数是一个函数，第二个是序列（列表 或元组）。其中，函数（即 map 的第一个参数位置的函数）可以接收一个或多个参数。

         reduce()第一个参数是函数，第二个是序列（列表或元组）。但是，其函数必须接收两个参数。

         (2) 从对传进去的数值作用来讲：

         map()是将传入的函数依次作用到序列的每个元素，每个元素都是独自被函数“作用”一次 。

         reduce()是将传人的函数作用在序列的第一个元素得到结果后，把这个结果继续与下一个元素作用（累积计算）。

     22. python中的yield的用法？

         yield 就是保存当前程序执行状态。你用 for 循环的时候，每次取一个元素的时候就会计算一次。用yield 的函数叫 generator，和 iterator 一样，它的好处是不用一次计算所有元素，而是用一次算一次，可以节省很多空间。generator每次计算需要上一次计算结果，所以用 yield，否则一 return，上次计算结果就没了。

     23.什么是猴子补丁？

     ​	在运行期间动态修改一个类或模块。

     24.python中协程的用法以及其底层原理？

     ​	https://zhuanlan.zhihu.com/p/330549526