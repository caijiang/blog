---
title: Maven OutOfMemoryError
date: 2013-04-11 16:47:34
categories: Maven
tags:
- Java
- JVM
---

在跑Maven Goal时可能会遭遇一些内存不足的问题。

可以尝试更变环境变量给予运行Maven的JVM更多内存空间以修复该问题。

在windows中可以尝试使用
{% highlight shell %}
set MAVEN_OPTS=-Xmx512m -XX:MaxPermSize=128m
{% endhighlight %}
或者在类Unix系统中用
{% highlight shell %}
export MAVEN_OPTS="-Xmx512m -XX:MaxPermSize=128m"
{% endhighlight %}
再重新跑Goal.
