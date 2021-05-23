---
title: CSS
date: 2021-3-31 21:09:00
categories: 前端基础
cover: /img/p18.jpg
tags: 博客
---



### CSS的使用方式

* 内联样式:在标签内使用style属性指定css代码

  ```html
  <div style="color:red;">hello css</div>
  ```

* 内部样式：在head标签内，定义style标签，style标签的标签体内容就是css代码

  ```html
  <style>
      div{
          color:blue;
      }
  </style>
  <div>hello css</div>
  ```

* 外部样式：

  * 定义css资源文件。
  * 在head标签内，定义link标签，引入外部的资源文件

  ```html
  CSS文件：
      div{
          color:green;
      }
  
  html代码：
      <link rel="stylesheet" href="css/a.css">
      <div>hello css</div>
      <div>hello css</div>
  ```

  **注意：**

  * 1,2,3种方式 css作用范围越来越大

  * 1方式不常用，后期常用2,3

  * 3种格式可以写为：

    ```
    <style>
        @import "css/a.css";
    </style>
    ```

### CSS语法

* 格式：

  ```
  选择器 {
      属性名1:属性值1;
      属性名2:属性值2;
      ...
  }
  ```

  选择器:筛选具有相似特征的元素

  每一对属性需要使用；隔开，最后一对属性可以不加；

### 选择器分类

* 基础选择器

  > * id选择器：选择具体的id属性值的元素.建议在一个html页面中id值唯一
  >
  >   语法：#id属性值{}
  >
  > * 元素选择器：选择具有相同标签名称的元素
  >
  >   语法： 标签名称{}
  >
  >   注意：id选择器优先级高于元素选择器
  >
  > * 类选择器：选择具有相同的class属性值的元素。
  >
  >   语法：.class属性值{}
  >
  >   注意：类选择器选择器优先级高于元素选择器

* 扩展选择器

  > * 选择所有元素：
  >
  >   语法： *{}
  >
  > * 并集选择器：
  >
  >   选择器1,选择器2{}
  >
  > * 子选择器：筛选选择器1元素下的选择器2元素
  >
  >   语法：  选择器1 选择器2{}
  >
  > * 父选择器：筛选选择器2的父元素选择器1
  >
  >   语法：  选择器1 > 选择器2{}
  >
  > * 属性选择器：选择元素名称，属性名=属性值的元素
  >
  >   语法：元素名称[属性名="属性值"]{}
  >
  > * 伪类选择器：选择一些元素具有的状态
  >
  >   语法： 元素:状态{}
  >
  >   如： <a>
  >
  >   状态：
  >
  >   * link：初始化的状态
  >   * visited：被访问过的状态
  >   * active：正在访问状态
  >   * hover：鼠标悬浮状态

### 选择器属性

* 字体、文字

  * font-size：字体大小
  * color：文本颜色
  *  text-align：对其方式
  * line-height：行高 

* 背景

  background

* 边框

  border:设置边框，符合属性

* 尺寸

  * width：宽度
  * height：高度

* 盒子模型

  * margin：外边距
  * padding：内边距
    * 默认情况下内边距会影响整个盒子的大小
    * box-sizing:border-box:设置盒子的属性，让width和height就是最终盒子的大小
  * float：浮动
    * left
    * right







