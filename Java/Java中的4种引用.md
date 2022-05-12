---
title: Java中的4种引用
date: 2022-05-12
tags: 
- Java
---

### 前言

在 JDK 1.2 之前，Java 中的引用的定义很传统：如果 reference 类型的数据中存储的数值代表的是另外一块内存的起始地址，就称该 refrence 数据是代表某块内存、某个对象的引用。这种定义很纯粹，但是太过狭隘，一个对象在这种定义下只有被引用或者没有被引用两种状态，对于如何描述一些“食之无味，弃之可惜”的对象就显得无能为力。

比如我们希望能描述这样一类对象：当内存空间还足够时，则能保留在内存之中；如果内存在进行垃圾收集后还是非常紧张，则可以抛弃这些对象。很多系统的缓存功能都符合这样的应用场景。

在 JDK 1.2 之后，Java 对引用的概念进行了扩充，将引用分为：

* 强引用（Strong Reference）
* 软引用（Soft Reference）
* 弱引用（Weak Reference）
* 虚引用（Phantom Reference）

这四种引用强度依次逐渐减弱。

### 1.强引用（Strong Reference）

java中的引用默认就是强引用，任何一个对象的赋值操作就产生了对这个对象的强引用。

我们看一个例子：

```java
public class StrongReferenceUsage {
    @Test
    public void stringReference(){
        Object obj = new Object();
    }
}
```

上面我们new了一个Object对象，并将其赋值给obj，这个obj就是new Object()的强引用。强引用的特性是只要有强引用存在，被引用的对象就不会被垃圾回收。

### 2.软引用（Soft Reference）

先看下SoftReference的定义：

```java
public class SoftReference<T> extends Reference<T>
```

SoftReference继承自Reference。它有两种构造函数：

```java
public SoftReference(T referent)
```

和：

```java
public SoftReference(T referent, ReferenceQueue<? super T> q)
```

软引用在java中有个专门的SoftReference类型，软引用的意思是只有在内存不足的情况下，被引用的对象才会被回收。

我们举个SoftReference的例子：

```java
@Test
    public void softReference(){
        Object obj = new Object();
        SoftReference<Object> soft = new SoftReference<>(obj);
        obj = null;
        log.info("{}",soft.get());
        System.gc();
        log.info("{}",soft.get());
    }
```

输出结果：

```text
22:50:43.733 [main] INFO com.flydean.SoftReferenceUsage - java.lang.Object@71bc1ae4
22:50:43.749 [main] INFO com.flydean.SoftReferenceUsage - java.lang.Object@71bc1ae4
```

可以看到在内存充足的情况下，SoftReference引用的对象是不会被回收的。

### 3.弱引用（weak Reference）

weakReference和softReference很类似，不同的是weekReference引用的对象只要垃圾回收执行，就会被回收，而不管是否内存不足。

同样的WeakReference也有两个构造函数：

```java
public WeakReference(T referent)；

 public WeakReference(T referent, ReferenceQueue<? super T> q)；
```

含义和SoftReference一致，这里就不再重复表述了。

我们看下弱引用的例子：

```java
@Test
    public void weakReference() throws InterruptedException {
        Object obj = new Object();
        WeakReference<Object> weak = new WeakReference<>(obj);
        obj = null;
        log.info("{}",weak.get());
        System.gc();
        log.info("{}",weak.get());
    }
```

输出结果：

```text
22:58:02.019 [main] INFO com.flydean.WeakReferenceUsage - java.lang.Object@71bc1ae4
22:58:02.047 [main] INFO com.flydean.WeakReferenceUsage - null
```

我们看到gc过后，弱引用的对象被回收掉了。

### 4.虚引用（PhantomReference）

PhantomReference的作用是跟踪垃圾回收器收集对象的活动，在GC的过程中，如果发现有PhantomReference，GC则会将引用放到ReferenceQueue中，由程序员自己处理，当程序员调用ReferenceQueue.pull()方法，将引用出ReferenceQueue移除之后，Reference对象会变成Inactive状态，意味着被引用的对象可以被回收了。

和SoftReference和WeakReference不同的是，PhantomReference只有一个构造函数，必须传入ReferenceQueue：

```java
public PhantomReference(T referent, ReferenceQueue<? super T> q)
```

看一个PhantomReference的例子：

```java
@Slf4j
public class PhantomReferenceUsage {

    @Test
    public void usePhantomReference(){
        ReferenceQueue<Object> rq = new ReferenceQueue<>();
        Object obj = new Object();
        PhantomReference<Object> phantomReference = new PhantomReference<>(obj,rq);
        obj = null;
        log.info("{}",phantomReference.get());
        System.gc();
        Reference<Object> r = (Reference<Object>)rq.poll();
        log.info("{}",r);
    }
}
```

运行结果：

```text
07:06:46.336 [main] INFO com.flydean.PhantomReferenceUsage - null
07:06:46.353 [main] INFO com.flydean.PhantomReferenceUsage - java.lang.ref.PhantomReference@136432db
```

我们看到get的值是null，而GC过后，poll是有值的。

因为PhantomReference引用的是需要被垃圾回收的对象，所以在类的定义中，get一直都是返回null：

```java
public T get() {
        return null;
}
```
