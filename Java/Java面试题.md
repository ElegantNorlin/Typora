---
title: Java面试题
date: 2021-3-28 16:05:00
tags: 
- Java
- Java面试题
toc: true
description: "亲手整理的面试题"
---



## Java面试题

### 1.hashtable和hashmap的区别

1 HashMap不是线程安全的

​      HashMap是map接口的子类，是将键映射到值的对象，其中键和值都是对象，并且不能包含重复键，但可以包含重复值。HashMap允许null key和null value，而hashtable不允许。

2  HashTable是线程安全。

HashMap是Hashtable的轻量级实现（非线程安全的实现），他们都完成了Map接口，主要区别在于HashMap允许空（null）键值（key）,由于非线程安全，效率上可能高于Hashtable。

HashMap允许将null作为一个entry的key或者value，而Hashtable不允许。 HashMap把Hashtable的contains方法去掉了，改成containsvalue和containsKey。因为contains方法容易让人引起误解。 Hashtable继承自Dictionary类，而HashMap是Java1.2引进的Map interface的一个实现。 最大的不同是，Hashtable的方法是Synchronize的，而HashMap不是，在多个线程访问Hashtable时，不需要自己为它的方法实现同步，而HashMap 就必须为之提供外同步。 Hashtable和HashMap采用的hash/rehash算法都大概一样，所以性能不会有很大的差别。

### 2.#{}和${}的区别

\#{} : 采用预编译方式,可以防止SQL注入
${}: 采用直接赋值方式,无法阻止SQL注入攻击

### 3.Java多线程实现的方式有四种

1.继承Thread类，重写run方法
2.实现Runnable接口，重写run方法，实现Runnable接口的实现类的实例对象作为Thread构造函数的target
3.通过Callable和FutureTask创建线程
4.通过线程池创建线程

### 4.Wait和sleep的区别

1、sleep是线程中的方法，但是wait是Object中的方法。

2、sleep方法不会释放lock，但是wait会释放，而且会加入到等待队列中。

3、sleep方法不依赖于同步器synchronized，但是wait需要依赖synchronized关键字。

4、sleep不需要被唤醒（休眠之后推出阻塞），但是wait需要（不指定时间需要被别人中断）。

### 5.IOC思想

IOC()控制反转，通俗的将就是我们吧设计好的对象交给IOC容器，我们需要的时候就从IOC容器中获取对象，不需要自己new对象。控制反转的意思理解：传统的对象的控制权在我们写的代码中，控制反转就是IOC容器来控制对象的产生。

### 6.List，set，map有什么区别？

List：

1.可以允许重复的对象。
2.可以插入多个null元素。
3.是一个有序容器，保持了每个元素的插入顺序，输出的顺序就是插入的顺序。
4.常用的实现类有 ArrayList、LinkedList 和 Vector。ArrayList 最为流行，它提供了使用索引的随意访问，而 LinkedList 则对于经常需要从 List 中添加或删除元素的场合更为合适。



Set：

1.不允许重复对象

2.无序容器，你无法保证每个元素的存储顺序，TreeSet通过 Comparator 或者 Comparable 维护了一个排序顺序。

3.只允许一个 null 元素
4.Set 接口最流行的几个实现类是 HashSet、LinkedHashSet 以及 TreeSet。最流行的是基于 HashMap 实现的 HashSet；TreeSet 还实现了 SortedSet 接口，因此 TreeSet 是一个根据其 compare() 和 compareTo() 的定义进行排序的有序容器。而且可以重复



Map:  

1.Map不是collection的子接口或者实现类。Map是一个接口。
2.Map 的 每个 Entry 都持有两个对象，也就是一个键一个值，Map 可能会持有相同的值对象但键对象必须是唯一的。

3.TreeMap 也通过 Comparator 或者 Comparable 维护了一个排序顺序。

4.Map 里你可以拥有随意个 null 值但最多只能有一个 null 键。
5.Map 接口最流行的几个实现类是 HashMap、LinkedHashMap、Hashtable 和 TreeMap。（HashMap、TreeMap最常用）



### 7.String引用类型的一些理解

```java
String s1 = "abc";
String s2 = "abc";
s1 == s2; # 结果为true


String s3 = new String("abc");
String s4 = new String("abc");
s3 == s4 
# 结果为false
# 原因：new的对象都是存在于堆中的，s3和s4的创建时new了两个对象存储在堆内存中，这两个对象的地址并不相同，所以结果时false。
```

### 8.朋友阿里实习一面试题

#### 8.1首先自我介绍一下

#### 8.2怎么解决订单下单的时候重复订单的问题

[放一个回答](https://baijiahao.baidu.com/s?id=1701803664665621483)

#### 8.3Maven如何排除包之间的冲突？

[放一个回答](https://blog.csdn.net/qq_39809613/article/details/103837979)

#### 8.4Threadlocal知识

#### 8.5数据库中的delete、update什么时候去释放锁？

#### 8.6说一些常用的设计模式：

#### 8.7适配器模式有几种模式实现：3钟实现方式

#### 8.8单例模式线程安全的实现方式

#### 8.9单例模式实现线程安全的方式对什么加锁：对类加锁还是对对象加锁

#### 8.10算法：leetcode136题

