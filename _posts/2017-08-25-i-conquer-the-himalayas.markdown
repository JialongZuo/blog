---
layout: post
title:  Object类源码分析
date:   2017-08-25 13:32:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: post-1.jpg # Add image post (optional)
tags: [JAVA, 源码学习]
author: 左佳龙
---

####概述

相信所有学习java的人都对Object这个类不陌生，它是类层级结构的根节点，所有类的父类（一切皆对象的具体体现？）
it is belived that all the people who coding in java are not familiar with the Class about Object.it the root of hierarchy and every class has Object as a superClass(maybe it is a embodiment of 'everything is object' )

####源码解析

Object类位于rt.jar(JAVA基础类库)中的java.lang包下。让我们看一下他的大体结构并逐个分析
Object is located library of rt.jar,and package in java.lang,let us overview it and then try to analysis it.
![Yosh Ginsu]({{site.baseurl}}/assets/img/object-overview.png)

```java
public class Object{

    //此方法用于注册一些原生方法
    //native是一个声明方法为本地方法的关键字，用于调用非java接口（如c/c++实现）。例如java本身不能操作系统底层，但可以通过JNI调用其他语言来实现此功能。
    private static native void registerNatives();

    //static所声明的静态代码块，再类被载入时执行。
    static {
        registerNatives();
    }

    //获取Object所对应的Class对象。
    //final关键字声明此方法不允许被覆写。
    public final native Class<?> getClass();

    //得到对象的hash值（固定长度的散列值）需要注意，同一个对象产生的hash值肯定是相同的而不同的对象产生的hash值不一定是不同的。所以不能通过hash值计算对象是否相同，但hash值不同那对象一定不相同。
    //hash值的使用场景是优化查找，先通过hash值进行分类，确定分类然后再精确查找。比如HashMap,HashTable等Hash集合中便于快速确定value位置。
    //JVM每次实例化一个Object时就会将其放入一个Hash表中来快速获取和比较对象。
    public native int hashCode();

    //对象比较方法，默认使用==操作符比较
    //==操作符比较对象的地址空间是否相同
    public boolean equals(Object obj) {
        return (this == obj);
    }

    //对象拷贝，开辟一块新的地址空间并将原对象数据拷贝（需要注意这里是浅拷贝）
    protected native Object clone() throws CloneNotSupportedException;

    //返回对象的Class名称+@+hash值（16进制）
    public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }

    //唤醒一个被该对象调用wait方法挂起的线程
    public final native void notify();

    //唤醒所有被该对象调用wait方法挂起的线程
    public final native void notifyAll();

    //暂停当前线程并释放对象锁
    public final native void wait(long timeout) throws InterruptedException;

    //释放一些无法通过CG回收的内存，比如通过native方法开辟的内存。
    protected void finalize() throws Throwable { }
}
```
> Snackwave chillwave seitan whatever, flannel wolf vinyl occupy activated charcoal succulents waistcoat. Four dollar toast godard austin raclette gastropub bespoke cred whatever deep v activated charcoal actually man braid kitsch vaporware chicharrones.

![Yosh Ginsu]({{site.baseurl}}/assets/img/yosh-ginsu.jpg)

