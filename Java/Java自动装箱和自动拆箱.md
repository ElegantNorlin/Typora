---
title: Java自动装箱和自动拆箱
date: 2021-3-28 16:05:00
categories: 
cover: /img/p15.jpg
tags: Java
---



首先我们要知道Java的装箱和拆箱操作是针对**基本类型**和**基本类型包装类**来说的两个概念。

先回顾一下基本类型及对应的包装类

| 基本数据类型 |  包装类   |
| :----------: | :-------: |
|     byte     |   Byte    |
|    short     |   Short   |
|     int      |  Integer  |
|     long     |   Long    |
|    float     |   Float   |
|    double    |  Double   |
|     char     | Character |
|   boolean    |  Boolean  |

### 自动装箱和拆箱

* 自动装箱：把基本数据类型转换为对应的包装类类型。
* 自动拆箱：把包装类类型转换为对应的基本数据类型。

```java
public class demo1 {
    public static void main(String[] args) {
        // 自动装箱：把int类型的100自动装箱成Integer类型的变量
        Integer i = 100;

        // 自动拆箱：i会自动拆箱（i.intValue()）为int类型的数值，然后与100相加
        int ii = i + 100;
    }
}

```

