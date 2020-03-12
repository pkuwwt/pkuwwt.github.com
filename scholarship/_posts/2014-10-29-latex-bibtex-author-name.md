---
layout: blog
comments: true
tags: LaTeX scholarship
title: Latex中bibtex的命名
---

## 英文名

事实上，bibtex中的名字其中区分四部分: first name, von, last name, Juniors。first name就是名，last name就是姓，last name是可以有多个单词的，但默认是一个。


bibtex中，波浪号会影响到bibtex的解析。比如`Per Brinch~Hansen`中的first name为Per，而last name为Brinch Hansen，中间的波浪号起了作用。如果写成`Per Brinch Hansen`，则bibtex会将其理解为`Per~Brinch Hansen`。


first name和last name都是靠首字线大写单词来区分的，也就是说von部分的首字线需要是小写。比如`Charles Louis Xavier Joseph de la Vallee Poussin`中的von部分为de la，first name有4个单词，last name有2个单词。


如果名字中有特殊字符，或有首字母小写的单词，需要用大括号来括起来。比如下面3个例子

  - {Barnes and Noble, Inc.}
  - {Barnes and} {Noble, Inc.}
  - {Barnes} {and} {Noble,} {Inc.}

注意相邻的两对大括号之间需要有空格。第一个只有一个last name，第二个有last name和first name，第三个四个部分是全的。


再比如

  - von Beethoven, Ludwig
  - {von Beethoven}, Ludwig


这两者是不同的，前者有von部分，last name是Beethoven，而后者没有von部分，"von Beethoven"是last name。


Juniors部分比较特殊，很多外国人喜欢在名字中的`Jr.`之前加逗号，比如`Ford, Jr., Henry`。但有些人则不用逗号。合理的处理方式是将`Jr.`作为first name或last name的一部分，比如下面两种方式的输出应该是一样的(没有Juniors部分)

  - {Steele Jr.}, Guy L
  - Guy L. {Steele Jr.}


总结一下，你只需要使用下面三种方式来在bibtex中书写英文姓名

  - First von Last
  - von Last, First
  - von Last, Jr, First

第一种方式最常用，但是如果有Juniors部分或没有von部分时，则考虑使用后两种。

## 中文拼音

如果是一个中国人的拼音按照英文的约定来，`Mao Zedong`一般要写成`Mao, Ze-Dong`，然后文献中的格式就变成了`Ze-Dong Mao`或`Z. D. Mao`。显然，并不是所有中国学者都按照遵守这个约定。

## first name的首字母缩写

另外一个问题是，如何让latex自动将first name首字母缩写呢，要么手动改成`Mao Z. D.`，注意有两个空格。要么自己写一个bst文件。具体步骤是

  - 将`texmf-dist/bibtex/bst/beebe/named.bst`拷到你的源码根目录下，改名为`abbrvnamed.bst`。
  - 找到函数`FUNCTION {format.names}`
  - 找到其中一行`{ s nameptr "{ff~}{vv~}{ll}{, jj}" format.name$ 't :=`
  - 将其改成`{ s nameptr "{f.~}{vv~}{ll}{, jj}" format.name$ 't :=`
  - 将源码中的bibtex风格改成`\bibliographystyle{abbrvnamed}`

这里的`ff`表示完整的first name, `f.`则是取首字母的first name，注意后面加了个点，`vv`表示`Von part`, `ll`表示last name, `jj`表示Juniors后缀。

另外，注意`named.bst`来源于`beebe`包，而使用`named.bst`又需要包`jneurosci`包。

### 参考文献
  - [only-authors-initials-in-bibtex-natbib-using-named-style](http://tex.stackexchange.com/questions/36660/only-authors-initials-in-bibtex-natbib-using-named-style)
  - [Bibtex Names](http://nwalsh.com/tex/texhelp/bibtx-23.html)

