---
comments: true
layout: blog
title: shell用法集锦
---

执行管道输出结果，一行一条命令，最笨的方法当然是存成文件，再执行。但也有不借助临时文件的两种方式
{% highlight bash %}
ls | sed 's/^/touch /g' | source /dev/stdin
source <(ls | sed 's/^/touch /g')
{% endhighlight %}

进入前一个当前目录
{% highlight bash %}
cd -
{% endhighlight %}

打印数字的16进制
{% highlight bash %}
print %x 11
print %x\\n 11
{% endhighlight %}

