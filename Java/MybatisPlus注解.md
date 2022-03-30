---
title: MybatisPlus注解
date: 2021-3-28 16:05:00
tags: Java
toc: true
description: "记录MybatisPlus注解"
---





* @TableId(type=IdType.***AUTO***)实现自增，其中type=IdType.***AUTO***为枚举值，我们直接使用即可。该注解标注在实体类的相关属性上。
* TableField(value="数据库表中的字段名"):该注解将数据库的字段和实体类中的属性相关联，一般在下面两种情况使用：
  * 对象中的属性名和字段不一致（也就是非驼峰，如果是驼峰就不需要加了会自动将字段与属性关联）
  * 对象中的属性字段在表中不存在的问题