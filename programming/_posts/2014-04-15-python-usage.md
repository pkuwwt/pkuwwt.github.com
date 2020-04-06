---
layout: blog
comments: true
tags: python programming
title: Python使用问题集锦
---

## Python的版本

```bash
python --version
```

```python
import sys
print(sys.version_info.major)
```


## 关于class中的成员变量
有一种成员变量需要特别小心，它们是直接声明在类定义中的成员变量，而非声明在`__init__`中。这种成员变量如果为对象类型(object)，则多个类实例会共享一个。


## 关于Warning
如果你想像Error一样捕捉Warning，可以使用`warnings`库

{% highlight python %}
import warnings
warnings.filterwarnings('error')
{% endhighlight %}

## 关于zip的逆过程
即将二元组列表，变成多个列表。

{% highlight python %}
zip(*[(1,2),(1,2),(1,2)])
zip(*[(1,2,3),(1,2,3),(1,2,3)])
{% endhighlight %}

得到的结果是元组的列表，比如`[(1,1,1),(2,2,2)]`。

## 判断操作系统是32位还是64位

{% highlight python %}
import struct
print(8*struct.calcsize('P'))
{% endhighlight %}

或`sys.maxsize > 2**32`, 但只适用于Python2.6以后。

## 合并多个list
{% highlight python %}
sum([[0,1],[2,3],[4,5]], [])
{% endhighlight %}

`sum`函数的第2个参数是初值，也就是将列表中的所有元素(也是列表)累加到这个初值上。

## itertools
获得1到9的数字字符的长度大于4的所有组合
{% highlight python %}
from itertools import *
iterlst = chain(*(permutation('123456789', i) for i in range(4,10)))
for i in iterlst:
    print i
{% endhighlight %}
`permutation`类的参数是一个"列表"和一个表示长度的整数，即由"列表"生成指定长度的所有排列。而`chain`类将多个"列表"合成一个"列表"。这里的"列表"实际上指的是可遍历对象(iterable)，即实现了`__iter__`方法的对象。`*`运算是将一个可遍历对象"解开"，即将其所有元素变成一个函数的参数。

无论是`chain`类还是`permutation`类，都是一次性的，访问一次之后就到底了。

`itertools.product`是另外一个有用的工具，它表示数学上的笛卡尔乘积，具体而言，是让两个列表中的元素构成所有可能的组合。
{% highlight python %}
import itertools
pairs = list(itertools.product([1,2,3],[4,5]))
print pairs.index((2,4))
{% endhighlight %}


## else
python中的`else`使用非常频繁，而且可以和`for`和`while`搭配使用，循环正常执行完成(没有`break`)时，会执行配套的`else`下的语句。

## list的list合并成一个list
比如将`l=[[1,2],[3,4],[5,6]]`变成`[1,2,3,4,5,6]`

{% highlight python %}
[item for sublist in l for item in sublist]
{% endhighlight %}

