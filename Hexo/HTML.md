---
title: HTML
date: 2021-3-30 16:04:00
categories: 前端基础
cover: /img/p16.png
tags: 博客
---

### HTML：Hyper Text Markup Language 超文本标记语言

* 超文本：超文本是用超链接的方法，将各种不同空间的文字信息组织在一起的网状文本
* 标记语言：由标签构成的语言。<标签名称> 如 html，xml。标记语言不是编程语言

### 知识补充

* html文档后缀名 .html 或者 .htm

* 标签分类：

  * 围堵标签：有开始标签和结束标签。如 <html> </html>
  * 自闭和标签：开始标签和结束标签在一起。如 <br/>

* 标签可以嵌套：需要正确嵌套，不能你中有我，我中有你

* 在开始标签中可以定义属性。属性是由键值对构成，值需要用引号(单双都可)引起来

* html的标签不区分大小写，但是建议使用小写。

  **代码示例：**

  ```html
  <html>
  		
      <head>
          <title>title</title>
      </head>
  
      <body>
          <FONT color='red'>Hello World</font><br/>
  
          <font color='green'>Hello World</font>
  
      </body>
  	
  </html>
  ```

### 标签学习

* 文件标签：构成html最基本的标签

  ```html
  html:html文档的根标签
  head：头标签。用于指定html文档的一些属性。引入外部的资源
  title：标题标签。
  body：体标签
  <!DOCTYPE html>：html5中定义该文档是html文档
  ```

* 文本标签：和文本有关的标签

  ```
  注释：<!-- 注释内容 -->
  <h1> to <h6>：标题标签
  h1~h6:字体大小逐渐递减
  <p>：段落标签
  <br>：换行标签
  <hr>：展示一条水平线
      属性：
          color：颜色
          width：宽度
          size：高度
          align：对其方式
          center：居中
          left：左对齐
          right：右对齐
  <b>：字体加粗
  <i>：字体斜体
  <font>:字体标签
  <center>:文本居中
      属性：
          color：颜色
          size：大小
          face：字体
  
  
  
  属性定义：
      color：
          1. 单词：red,green,blue...
          2. rgb(值1，值2，值3)：值的范围：0~255  如  rgb(0,0,255)
          3. #值1值2值3：值的范围：00~FF之间。如： #FF00FF
      * width：
          1. 数值：width='20' ,数值的单位，默认是 px(像素)
          2. 数值%：占比相对于父元素的比例
  ```

* 表单标签：用于采集用户输入的数据的。用于和服务器进行交互。

  ```html
  示例：
  <form action="#" method="get">
      ......
  </form>
  ```

  * form：用于定义表单的。可以定义一个范围，范围代表采集用户数据的范围

  * 属性：

    * action：指定提交数据的URL

    * method：指定提交的方式: 共有7中方式，常用的有两种：

      > * get:
      >
      >   1. 请求参数会在地址栏中显示。会封装到请求行中(HTTP协议后讲解)。
      >   2. 请求参数大小是有限制的。
      >   3. 不太安全。
      >
      > * post:
      >
      >   1.请求参数不会再地址栏中显示。会封装在请求体中(HTTP协议后讲解)
      >
      >   2.请求参数的大小没有限制。
      >
      >   3.较为安全。

  * 表单项中的数据要想被提交：必须指定其name属性

  ```html
  表单项标签：
  input：可以通过type属性值，改变元素展示的样式
      type属性：
      text：文本输入框，默认值
          placeholder：指定输入框的提示信息，当输入框的内容发生变化，会自动清空提示信息	
      password：密码输入框
      radio:单选框
          注意：
              1. 要想让多个单选框实现单选的效果，则多个单选框的name属性值必须一样。
              2. 一般会给每一个单选框提供value属性，指定其被选中后提交的值
              3. checked属性，可以指定默认值
      checkbox：复选框
          注意：
          1. 一般会给每一个单选框提供value属性，指定其被选中后提交的值
          2. checked属性，可以指定默认值
      file：文件选择框
      hidden：隐藏域，用于提交一些信息。
      按钮：
          submit：提交按钮。可以提交表单
          button：普通按钮
          image：图片提交按钮
              src属性指定图片的路径	
  
  label：指定输入项的文字描述信息
      注意：
          label的for属性一般会和 input 的 id属性值 对应。如果对应了，则点击label区域，会让input输入框获取焦点。
  
  select: 下拉列表
      子元素：option，指定列表项
  
  textarea：文本域
      cols：指定列数，每一行有多少个字符
      rows：默认多少行。
  ```

  

