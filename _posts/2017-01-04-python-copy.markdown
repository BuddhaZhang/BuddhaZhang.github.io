---
layout:     post
title:      "python中的浅拷贝和深拷贝"
subtitle:   "几个简单的例子搞清楚python中的浅拷贝和深拷贝"
date:       2017-01-04
author:     "Michael Zhang"
header-img: "img/post-bg-2015.jpg"
tags:
    - python
---



# 正文

在阅读《机器学习实战》一书中，发现其中的python代码不少地方还是可以改进的。

比如在很多地方需要拷贝变量时（因为要对传入函数的变量进行修改，但又不想原来的变量被改变），书中的做法大都是这样：

```python
dataSet = [1,23,456]
newDataSet = dataSet[:]
```

然而这种方法是有问题的，最好还是使用copy模块中deepcopy函数。

至于为什么，接着看下面的几个简单例子。


#### python 中 对象分为
+ 不可变（immutable）：int、float、（数值型number）、字符串(string)、元组（tuple)
+ 可变（mutable）：字典型(dictionary)、列表型(list)

其实本质区别就是变量存的是值还是引用（C中的指针），或者说传参数时传值还是指针


```python
a = 1
b = 1
print id(a), id(b)
a += 2
print id(a), id(b)
```

    30571192 30571192
    30571144 30571192
    

可以看到int 1这个内存地址一直没变（引用计数>0，未达到销毁条件）


```python
list_a = [1, 'abc',[1,2,3],(5,6)]
list_b = list_a        # 复制引用 (赋值)
list_c = list_a[:]      # 完全切片
list_d = list(list_a)  # 工厂方法，list_b, list_c 其实都等同于copy模块中的copy方法得到的list = copy.copy(list_a)
```


```python
from copy import deepcopy
list_e = deepcopy(list_a)  # 深拷贝
```


```python
list_a[0] = -1     #  改变列表的元素
print list_a, list_b, list_c, list_d, list_e
```

    [-1, 'abc', [1, 2, 3], (5, 6)] [-1, 'abc', [1, 2, 3], (5, 6)] [1, 'abc', [1, 2, 3], (5, 6)] [1, 'abc', [1, 2, 3], (5, 6)] [1, 'abc', [1, 2, 3], (5, 6)]
    

改变复制之源的列表的元素后，发现浅拷贝的内容和深拷贝一样未变。真的是这样吗


```python
list_a[2][2] = -3
print list_a, list_b, list_c, list_d, list_e
```

    [-1, 'abc', [1, 2, -3], (5, 6)] [-1, 'abc', [1, 2, -3], (5, 6)] [1, 'abc', [1, 2, -3], (5, 6)] [1, 'abc', [1, 2, -3], (5, 6)] [1, 'abc', [1, 2, 3], (5, 6)]
    

再次改变复制之源的列表的元素后，发现浅拷贝的内容竟然也发生了变化


```python
list_a[3] = (-5, -6)
print list_a, list_b, list_c, list_d, list_e
```

    [-1, 'abc', [1, 2, -3], (-5, -6)] [-1, 'abc', [1, 2, -3], (-5, -6)] [1, 'abc', [1, 2, -3], (5, 6)] [1, 'abc', [1, 2, -3], (5, 6)] [1, 'abc', [1, 2, 3], (5, 6)]
    


```python
print [id(x) for x in list_a]
print [id(x) for x in list_b]
print [id(x) for x in list_c]
print [id(x) for x in list_d]
print [id(x) for x in list_e]
```

    [30571240L, 44812888L, 66124552L, 70299016L]
    [30571240L, 44812888L, 66124552L, 70299016L]
    [30571192L, 44812888L, 66124552L, 46205384L]
    [30571192L, 44812888L, 66124552L, 46205384L]
    [30571192L, 44812888L, 67655240L, 46205384L]
    

发现5个列表第二个元素地址都一样，因为从未动过，它们的第二个元素指向的都是同一块内存位置<br>
第一个元素（int）和第四个元素（tuple），三个拷贝得到的对象位置和复制之源的不一样。<br>
第三个元素（list），只有深拷贝和复制之源的不一样<br>

<br><br>
所以结论是什么？<br>

复制引用的结果是新对象所有属性都和原来的同步<br>

浅拷贝的结果是新对象的不变属性（此处当然指第一层，不可能指对象的某一个属性是list，list中的一个元素是int这种不变类型）和原来的不同步，可变属性和原来的同步变化<br>

深拷贝的结果是新对象的所有属性都和原来的不同步变化
