---
title: Java如何在运行时获知自身Jar的版本
date: 2013-05-09 10:30:52
categories: Java
tags:
- Java
- Maven
---

按照Java Specification应当可以通过
{% highlight ruby %}
getClass().getPackage().getImplementationVersion()
{% endhighlight %}
或者
{% highlight ruby %}
getClass().getPackage().getSpecificationVersion()
{% endhighlight %}

实际上，这个值是依赖Java Jar规范的manifest信息。

这个[链接](http://docs.oracle.com/javase/8/docs/technotes/guides/jar/jar.html#Manifest_Specification)是这个信息的详细规范。
