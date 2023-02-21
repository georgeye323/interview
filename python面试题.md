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

   *  经典类目前在Python里基本没有应用、

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

     