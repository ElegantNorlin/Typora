---
title: Threadlocal及其内存泄漏原因
date: 2021-3-28 16:05:00
tags: Java
toc: true
description: ""
---





## Threadlocal及其内存泄漏原因

### 先来说说Threadlocal是什么

**Threadlocal是为了解决对象不能被多线程共享访问的问题**，通过 threadLocal.set() 方法将对象实例保存在每个线程自己所拥有的 threadLocalMap 中，这样的话每个线程都使用自己的对象实例，彼此不会影响从而达到了隔离的作用，这样就解决了对象在被共享访问时带来的线程安全问题。

啥意思呢?打个比方，现在公司所有人都要填写一个表格，但是只有一支笔，这个时候就只能上个人用完了之后，下个人才可以使用，为了保证"笔"这个资源的可用性，只需要保证在接下来每个人的获取顺序就可以了，这就是 lock 的作用，当这支笔被别人用的时候，我就加 lock ，你来了那就进入队列排队等待获取资源(非公平方式那就另外说了)，这支笔用完之后就释放 lock ，然后按照顺序给下个人使用。

但是完全可以一个人一支笔对不对，这样的话，你填写你的表格，我填写我的表格，咱俩谁都不耽搁谁。这就是 ThreadLocal 在做的事情，因为每个 Thread 都有一个副本，就不存在资源竞争，所以也就不需要加锁，这不就是拿空间去换了时间嘛！

接下来我们讲一下线程Thread中持有ThreadLocalMap，ThreadLocalMap 又是由 Entry 来组成的，在 Entry 里面有 ThreadLocal 和 value。其中Entry相当于键值对的数据结构，ThreadLocalMap结构比较特殊，其中存储的是Entry。

Threadlocal为弱引用，在下一次垃圾回收时ThreadLocalMap就会被回收掉。



### 聊一聊ThreadLocalMap造成内存泄漏的原因

首先是因为 ThreadLocal 是基于 ThreadLocalMap 实现的，其中 ThreadLocalMap 的 Entry 继承了 WeakReference ，而 Entry 对象中的 key 使用了 WeakReference 封装，也就是说， Entry 中的 key 是一个弱引用类型，对于弱引用来说，它只能存活到下次 GC 之前

如果此时一个线程调用了 ThreadLocalMap 的 set 设置变量，当前的 ThreadLocalMap 就会新增一条记录，但由于发生了一次垃圾回收，这样就会造成一个结果: key 值被回收掉了，但是 value 值还在内存中，而且如果线程一直存在的话，那么它的 value 值就会一直存在

这样被垃圾回收掉的 key 就会一直存在一条引用链: Thread -> ThreadLocalMap -> Entry -> Value就是因为这条引用链的存在，就会导致如果 Thread 还在运行，那么 Entry 不会被回收，进而 value 也不会被回收掉，但是 Entry 里面的 key 值已经被回收掉了

这只是一个线程，如果再来一个线程，又来一个线程…多了之后就会造成内存泄漏



### 解决方法

知道是怎么造成内存泄漏之后，接下来要做的事情就好说了，不是因为 value 值没有被回收掉所以才会导致内存泄露的嘛

那使用完 key 值之后，将 value 值通过 remove 方法 remove 掉，这样的话内存中就不会有 value 值了，也就防止了内存泄漏嘛
