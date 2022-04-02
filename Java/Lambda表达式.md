---
title: Lambda表达式
date: 2022-04-02
tags: 
- Java
---



### 1.为什么要使用Lambda表达式？

#### 1.1使用匿名内部类存在的问题

需求：需要启动一个线程去完成任务时，通常会通过 传统写法,代码如下：`Runnable`接口来定义任务内容，并使用

`Thread`类来启动该线程。代码如下：

```java
public class Demo01LambdaIntro {

  public static void main(String[] args) {
    new Thread(new Runnable() {
      @Override public void run() {
        System.out.println("新线程任务执行！"); 
      } 
    }).start();
  }
}
```

由于面向对象的语法要求，首先创建一个`Runnable`接口的匿名内部类对象来指定线程要执行的任务内容，再将其交给一个线程来启动。

对于`Runnable`的匿名内部类用法，可以分析出几点内容： 

* `Thread`类需要 `Runnable`接口作为参数，其中的抽象`run`方法是用来指定线程任务内容的核心 
* 为了指定 `run` 的方法体，不得不需要 `Runnable` 接口的实现类 
* 为了省去定义一个 `Runnable` 实现类的麻烦，不得不使用匿名内部类 
* 必须覆盖重写抽象 `run` 方法，所以方法名称、方法参数、方法返回值不得不再写一遍，且不能写错，而实际上，似乎只有方法体才是关键所在。

#### 1.2Lambda解决匿名内部类问题

Lambda是一个匿名函数。

借助Java 8的全新Lambda表达式语法，上述Runnable接口的匿名内部类写法可以通过更简单的Lambda表达式达到相同的效果

```java
public class Demo01LambdaIntro {
  public static void main(String[] args) {
    new Thread(() -> System.out.println("新线程任务执行！")).start(); // 启动线程 
  } 
}
```

这段代码和刚才的执行效果是完全一样的，可以在JDK 8或更高的编译级别下通过。从代码的语义中可以看出：我们启动了一个线程，而线程任务的内容以一种更加简洁的形式被指定。我们只需要将要执行的代码放到一个Lambda表达式中，不需要定义类，不需要创建对象。

### 2.Lambda的标准格式

使用Lambda表达式的前提是函数式接口，Lambda是依赖与函数式接口的。

Lambda的标准格式由3部分组成：

```java
(参数类型 参数名称) -> {
  代码体;
}
```

#### 2.1Lambda与method的对比

* 匿名内部类

  ```java
  public void run() {
    System.out.println("aa");
  }
  ```

* Lambda

  ```java
  () -> System.out.println("bb");
  ```

#### 2.2无参数、无返回值的Lambda

```java
//定义游泳接口
interface Swimmable {
  //定义函数式接口中唯一的抽象方法
  public abstract void swimming();
}
```



```java
package com.itheima.demo01lambda;
//编写测试类
public class Demo02LambdaUse {
	//程序入口
  public static void main(String[] args) {
    //使用匿名内部类的方式重写抽象方法
    goSwimming(new Swimmable() {
      @Override
      public void swimming() {
        System.out.println("匿名内部类游泳"); 
      }
    });
		//使用Lambda表达式实现匿名内部类达到的效果
    goSwimming(() -> {
      System.out.println("Lambda游泳");
    });
	}
  
  //定义游泳方法，参数需要游泳接口
  public static void goSwimming(Swimmable swimmable) {
    //调用游泳接口的抽象方法
    //由于在该方法中，接口并没有实现类，也就没有重写方法。那么必须在使用方式时重写方法。
    swimmable.swimming();
  }
}
```

#### 2.3有参数的有返回值的Lambda

当需要对一个对象集合进行排序时，Collections.sort方法需要一个Comparator接口实例来指定排序的规则，所以需要实现Comparator函数式接口。

我们分别使用匿名内部类和Lambda表达式来实现

```java
//匿名内部类
Collections.sort(persons, new Comparator<Person>() {
  @Override 
  public int compare(Person o1, Person o2) {
    return o1.getAge() - o2.getAge();
  } 
});

//Lambda
Collections.sort(persons, (o1, o2) -> {
  return o1.getAge() - o2.getAge();
});
```



### 3.Lambda省略规则

* 小括号内的参数类型可以省略；
* 如果小括号内有且仅有一个参数，则小括号可以省略；
* 如果大括号内有且仅有一个语句，可以同时省略大括号、return关键字及语句分号；

### 4.Lambda前提条件

* 必须是函数式接口：有且仅有一个抽象方法的接口

### 5.总结

Lambda表达式通俗的来说就是来简化匿名内部类的书写，并且其只能重写函数式接口。这个函数式接口可以在方法的`参数部分`、方法的`返回值`、甚至是`变量`。



