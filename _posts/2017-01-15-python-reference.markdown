---
layout:     post
title:      "python引用类型深入探究"
subtitle:   ""
date:       2017-01-15
author:     "Michael Zhang"
header-img: "img/post-bg-2015.jpg"
tags:
    - python
---



## 结论

结论先放前面：
+ python中变量皆引用（1，"abc",(2,3)这些不变类型在内存中是不可以改变的，a=1，a变量存的是1的地址）
+ 函数也是一个对象


## 正文


### 不要混淆了不变类型与可变类型和值传递与引用传递

```python
a = 3; b = 3; c = 4
print id(a),id(b),id(c)
```

    6580104 6580104 6580080
    


```python
a =4
print id(a),id(b),id(c)
```

    6580080 6580104 6580080
    


```python
print id(3)
```

    6580104
    

## 以上说明了什么：python里面变量都是引用类型，没有值类型。所以int变量存的都是int的引用。


```python
string_a = "abc"; string_b = "abc"; string_c = "abcd"
print id(string_a), id(string_b), id(string_c)
```

    45632208 45632208 74546472
    


```python
print id("abc"), id("abcd")
```

    45632208 74546472
    

## 字符串和int变量类似，也是相同的字符串的内存地址都是一样的


```python
tuple_a =(1,2);tuple_b = (1,2); tuple_c =(2,3)
print id(tuple_a), id(tuple_b), id(tuple_c)
```

    50464328 72726216 50465544
    


```python
tuple_a = (2,3)
print id(tuple_a), id(tuple_b), id(tuple_c)
```

    50466632 72726216 50465544
    


```python
print id((1,2))
```

    50542920
    

## 元组的实现和int，string不太一样


```python
list_a =[1,2]; list_b = [1,2];list_c = [2,3]
print id(list_a), id(list_b), id (list_c)
```

    72812168 50535240 72812488
    


```python
list_a[0]=2;list_a[1] =3
print id(list_a)
```

    72812168
    

## 从上面的结果可以看出list_a的引用地址并没有发生变化

## 在下面这个python代码中。fs是一个列表，fs的每个元素都是一个lambda表达式


```python
fs = [(lambda n: i + n) for i in range(10)]
print fs[3](4)
```

    13
    


## 使用fs中的第四个元素来当函数使用，结果却预期的不太一样。为什么


```python
print fs[0](4), fs[1](4),fs[2](4),fs[3](4)
```

    13 13 13 13
    

## 看来这几个lambda表达式用得是同一个i变量，当i=9过后，大家就都一样了


```python
fs2 = [(lambda n,i=i: i + n) for i in range(10)]
print fs2[3](4)
```

    7
    

## 这一次的结果符合我们想要的结果了。为什么：因为在lambda里面声明了一个本地变量

## 真正pythonic的方式是这样的


```python
fs3 = map(lambda i:lambda n:i+n, range(10))
print fs3[3](4)
```

    7
    
## 函数为什么能够存在列表里面呢，看下面

```python
def myFunction():
    print "whoops"
print dir(myFunction)
```

    ['__call__', '__class__', '__closure__', '__code__', '__defaults__', '__delattr__', '__dict__', '__doc__', '__format__', '__get__', '__getattribute__', '__globals__', '__hash__', '__init__', '__module__', '__name__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'func_closure', 'func_code', 'func_defaults', 'func_dict', 'func_doc', 'func_globals', 'func_name']
    


## 可以新增函数的属性
```python
myFunction.nameValue = "who konws"
print myFunction.nameValue
```

    who konws
    
## 对象的__call__方法
```python
class MyClass:
    pass
myObject = MyClass()
myObject.__call__ = (lambda x: x ** 2)
```


```python
print myObject(13)
```

    169
    


```python
print dir(myObject)
```

    ['__call__', '__doc__', '__module__']
    

## 发现了什么没有。函数也是一个对象，所以在高阶函数中才能传入函数，返回函数


```python
print dir(myObject.__call__)
```

    ['__call__', '__class__', '__closure__', '__code__', '__defaults__', '__delattr__', '__dict__', '__doc__', '__format__', '__get__', '__getattribute__', '__globals__', '__hash__', '__init__', '__module__', '__name__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'func_closure', 'func_code', 'func_defaults', 'func_dict', 'func_doc', 'func_globals', 'func_name']
    


```python
print dir(myObject.__call__.__str__)
```

    ['__call__', '__class__', '__cmp__', '__delattr__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__name__', '__new__', '__objclass__', '__reduce__', '__reduce_ex__', '__repr__', '__self__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
    

