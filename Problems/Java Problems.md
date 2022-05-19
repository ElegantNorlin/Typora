---
title: Java Problems
date: 2022-05-19
tags: 
- Java
- Problems
---





### 1.`||`的使用细节

先来看一段代码

```java
//head为指针

//第一种
if(head == null || head.next == null){
		return head;
}

//第二种
if(head.next == null || head == null){
		return head;
}
```

这两段代码选择一段合适的使用，你会选择哪种呢？

第二种容易造成空指针异常：

当head = null时，使用第二种就会空指针异常，而使用第一种程序就会正常的执行下去。

