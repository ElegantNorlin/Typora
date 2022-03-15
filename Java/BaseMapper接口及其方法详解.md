---
title:BaseMapper接口及其方法详解
date: 2021-3-28 16:05:00
categories: 
cover: /img/p15.jpg
tags: Java
---



```
insert
deleteById
deleteByMap
delete
deleteBatchIds
updateById
update
selectById
selectBatchIds
selectByMap
selectOne
selectCount
selectList
selectMaps
selectObjs
selectPage
selectMapsPage
```



**BaseMapper接口是Mybatis Plus提供的单表操作接口，只能用来操作单表。**

```java
/**
 * Mapper 继承该接口后，无需编写 mapper.xml 文件，即可获得CRUD功能
 * 这个 Mapper 支持 id 泛型
 * @author hubin
 * @Date 2016-01-23
 */
public interface BaseMapper<T> {
    /**
     * 插入一条记录
     * @param entity 该参数为实体对象T
     * @return int
     */
    Integer insert(T entity);
 
    /**
     * 根据传入的 ID 进行删除操作
     * @param id
     * 主键ID
     * @return int
     */
    Integer deleteById(Serializable id);
 
    /**
     * 根据 columnMap 条件，删除记录
     * @param columnMap
     * 表字段 map 对象
     * @return int
     */
    Integer deleteByMap(Map<String, Object> columnMap);
 
    /**
     * 根据 entity 条件，删除记录
     * @param wrapper
     * 实体对象封装操作类（可以为 null）
     * @return int
     */
    Integer delete(Wrapper<T> wrapper);
 
    /**
     * 删除（根据ID 批量删除）
     * @param idList
     * 主键ID列表
     * @return int
     */
    Integer deleteBatchIds(List<? extends Serializable> idList);
 
    /**
     * 根据 ID 修改
     * @param entity为设置了id的实体类，entity对象设置的参数为要更新的字段
     * @return int
     */
    Integer updateById(T entity);
 
    /**
     * 根据 whereEntity 条件，更新记录
     * @param entity
     * 实体对象
     * @param wrapper
     * 实体对象封装操作类（可以为 null）
     * @return
     */
    Integer update(T entity, @Param("ew") Wrapper<T> wrapper);
  	
  
  	# 以下两种更新方法达到的目的是相同的
    @Test
    public void testUpdate1(){
      User user = new User();
      user.setAge(20);
      user.setPassword("8888888");
      QueryWrapper<User> wrapper = new QueryWrapper<>();
      //设置条件为user_name="zhangsan"
      wrapper.eq("user_name",zhangsan);
      
      //根据条件（这里是用user_name字段作为条件）做更新
      int result = this.userWrapper.update(user,wrapper);
    }
  
  	@Test
    public void testUpdate2(){
      UpdateWrapper<User> wrapper = new UpdateWrapper<>();
      wrapper.set("age",20).set("password",999999999).eq("user_name","zhangsan");

      //我们不需要设置实体对象，所以第一个参数设置为null，wrapper包含了“更新数据”和“更新条件”
      int result = wrapper.update(null,wrapper);
    }
 
    /**
     * 根据 ID 查询
     * @param id
     * 主键ID
     * @return T
     */
    T selectById(Serializable id);
 
    /**
     * 查询（根据ID 批量查询）
     * @param idList
     * 主键ID列表
     * @return List<T>
     */
    List<T> selectBatchIds(List<? extends Serializable> idList);
 
    /**
     * 查询（根据 columnMap 条件）
     * @param columnMap
     * 表字段 map 对象
     * @return List<T>
     */
    List<T> selectByMap(Map<String, Object> columnMap);
 
    /**
     * 根据 entity 条件，查询一条记录
     * @param entity
     * 实体对象
     * @return T
     */
    T selectOne(T entity);
 
    /**
     * 根据 Wrapper 条件，查询总记录数
     * @param wrapper
     * 实体对象
     * @return int
     */
    Integer selectCount(Wrapper<T> wrapper);
 
    /**
     * 根据 entity 条件，查询全部记录
     * @param wrapper
     * 实体对象封装操作类（可以为 null）
     * @return List<T>
     */
    List<T> selectList(Wrapper<T> wrapper);
 
    /**
     * 根据 Wrapper 条件，查询全部记录
     * @param wrapper
     * 实体对象封装操作类（可以为 null）
     * @return List<T>
     */
    List<Map<String, Object>> selectMaps(Wrapper<T> wrapper);
 
    /**
     * 根据 Wrapper 条件，查询全部记录
     * @param wrapper
     * 实体对象封装操作类（可以为 null）
     * @return List<Object>
     */
    List<Object> selectObjs(Wrapper<T> wrapper);
 
    /** 
     * 用法：(new RowBounds(offset, limit), ew);
     * 根据 entity 条件，查询全部记录（并翻页）
     * @param rowBounds
     * 分页查询条件（可以为 RowBounds.DEFAULT）
     * @param wrapper
     * 实体对象封装操作类（可以为 null）
     * @return List<T>
     */
    List<T> selectPage(RowBounds rowBounds, Wrapper<T> wrapper);
 
    /** -- 不常用,
     * 根据 Wrapper 条件，查询全部记录（并翻页）
     * @param rowBounds
     * 分页查询条件（可以为 RowBounds.DEFAULT）
     * @param wrapper
     * 实体对象封装操作类
     * @return List<Map<String, Object>>
     */
    List<Map<String, Object>> selectMapsPage(RowBounds rowBounds, Wrapper<T> wrapper);
}
```



