## Resouces
-[1天搞定SpringBoot+Vue全栈开发](https://www.bilibili.com/video/BV1nV4y1s7ZN/)

## Spring Boot Full Stack

### Spring Boot

###### IDEA实现热启动
-TO BE DONE


###### 使用Swagger 创建Restful风格API
1.添加依赖
swagger2  swagger-ui
2.Apioperation（''）
3.配置类
4.访问http://localhost:8080/swagger-ui.html

###### ORM
ORM(Object Relational Mapping)对象关系映射
解决面向对象和关系型数据库之间的不匹配问题
MyBatis 数据持久层的框架

依赖：
1.MyBatis
2.Mysql
3.druid

###### Mybatis-plus
如果查询的数据库里不存在该字段，则加上注解：
@TableField(exist = false)

对于外表查询，可以使用：
[多表查询](E:\MyGithubWeb\SpringBoot-FullStack\外表查询.png)

以用户和订单为例。
一个用户对应多个订单，一个订单对应唯一一个用户。

在用户的实体类中，有orders这样的字段，但是数据库中并没有。可以通过
在用户中查询该用户的所有订单，可以在UserMapper中这样写:
查询所有用户和所有订单
@Select("select * from t_user)
@Results(
    {
        @Result(column="id", property="id"),
        @Result(column="username", property="username"),
        @Result(password="password", property="password"),
        @Result(birthday="birthday", property="birthday"),
        @Result(column="id", property="orders",
            many=@Many(select="com.example.demo.mapper.OrderMapper.selectByuid")
        )
    }
)
List<User> selectAllUserAndOrders();
调用OrderMapper中的selectByuid方法，传入用户的id，返回该用户的所有订单。

再来看看OrderMapper
@Select("select * from t_order where uid=#{uid}")
List<Order> selectByuid(int uid);
查询出来的值会交给上面的orders属性，完成赋值。

总结一下：查询所有用户及他们自己的订单，首先在User实体类中加入属性orders，然后在UserMapper中写一个查询所有用户及他们的订单的方法，再在OrderMapper中写一个查询某个用户的所有订单的方法。

条件查询 QueryWrapper
@GETMapping("/user/find")
public List<User> findByCond(){
    QueryWrapper<User> queryWrapper= new QueryWrapper();
    queryWrapper.eq("username","zhangsan");
    return userMapper.selectList(queryWrapper);
}

分页查询：
1.
@GETMapping("/user/findByPage")
public IPage findByPage(){
    Page<User> page = new Page<>(0,2);
    IPage ipage = userMapper.selectPage(page,null);
    return ipage;
}
2.在配置类中
添加分页拦截器

