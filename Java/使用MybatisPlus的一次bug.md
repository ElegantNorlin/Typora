

记一次使用MybatisPlus出现的问题，在使用last方法实现limit功能的时候，字符串的最后一定要留空格，不然一定会报错的。

```java
queryWrapper.last("limit "+limit);
```

