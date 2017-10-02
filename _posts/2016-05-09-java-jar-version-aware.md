---
title: Java如何在运行时获知自身Jar的版本
comment: true
date: 2016-05-09 10:30:52
categories: Java
tags:
- Java
- Maven
---

按照Java Specification应当可以通过
{% highlight java %}
getClass().getPackage().getImplementationVersion()
{% endhighlight %}
或者
{% highlight java %}
getClass().getPackage().getSpecificationVersion()
{% endhighlight %}

实际上，这个值是依赖Java Jar规范的manifest信息。

这个[链接](http://docs.oracle.com/javase/8/docs/technotes/guides/jar/jar.html#Manifest_Specification)是这个信息的详细规范。

显然每次打包的时候手工写入这个文件是件坑爹的事情，我们需要将这个业务工程化，而且Maven已经做到了这点。

非常简单的添加：
{% highlight maven %}
    <plugin>
        <artifactId>maven-jar-plugin</artifactId>
        <configuration>
            <archive>
                <manifest>
                    <addDefaultImplementationEntries>true</addDefaultImplementationEntries>
                    <addDefaultSpecificationEntries>true</addDefaultSpecificationEntries>
                </manifest>
            </archive>
        </configuration>
    </plugin>
{% endhighlight %}
到我们pom.xml的build模块即可。

package it & enjoy it!
