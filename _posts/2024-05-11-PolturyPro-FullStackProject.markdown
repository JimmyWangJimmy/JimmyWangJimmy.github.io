

开发流程：
1.明确需求，设计数据库表结构
2.开发后端接口
3.进行前端UI设计
4.开发前端
5.测试
6.交由使用人员使用
7.完成项目文档，提交申请软著专利。

目前进行：2.开发后端接口 4.开发前端

待解决问题：1.后端条件查询映射 user_id userId


解决SpringBoot2.7整合Swagger报错。
报错信息：
```
org.springframework.context.ApplicationContextException: Failed to start bean 'documentationPluginsBootstrapper'; nested exception is java.lang.NullPointerException
```
解决方式：在`application.properties`中添加配置
```properties
spring.mvc.pathmatch.matching-strategy=ant_path_matcher
```
启动项目，输入以下网址查看接口文档：
http://localhost:8888/swagger-ui/index.html



###### SpringBoot properties配置
```properties
//定义过滤规则
spring.mvc.static-path-pattern=/images/**
//定义静态资源路径
spring.web.resources.static-locations=classpath:/static/
```

###### 文件上传
表单格式选用form-data 
使用apifox进行测试，参数nickname(string)，photo(file)



###### SpringBoot整合Mybatis-Plus

添加依赖：
```xml
        <!--        引入mybatis-plus-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.2</version>
        </dependency>
        <!--        引入mysql驱动依赖-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <!--        引入数据库连接池druid-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.20</version>
        </dependency>
```
进行配置：

```
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/poultryprodb?userSSL=false
spring.datasource.username=root
spring.datasource.password=1234
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```
最后创建mapper包，并在启动类上添加注释：
```java
@MapperScan("com.example.poultrypro.mapper")
```

新建数据库PoultryProDB 
设置字符集为utf8mb4，排序规则为utf8mb4_general_ci
在启动类添加@MapperScan注解


创建Mapper接口,MyBatis-Plus进一步简化了MyBatis，继承BaseMapper接口，传入实体类名称，即可实现增删改查。
```java
/*
 * @Description:利用Mybatis-Plus简化开发，继承BaseMapper,传入实体类名称，实现增删改查。
 */
@Mapper
public interface PurchaseRecordMapper extends BaseMapper<PurchaseRecord> {

}

通过注解，解决实体类名与数据库名不一致。
```java
@TableName("purchase_record")
public class PurchaseRecord {
    //
}
```
通过添加@TableId注解设置自增主键。
```java
    @TableId(type = IdType.AUTO)
    private int user_id;
```
添加@TableField，解决实体类与数据库非主键字段名不一致。
```java
    @TableField(value = "user_name")
    private String userName;
```


##### JWT登录认证
加入依赖：
```xml
        <!--        引入jwt-->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.1</version>
        </dependency>
```
生成32位secret：a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6

###### 解决数据库中文显示问题
数据库设置位utf8mb4，排序规则为utf8mb4_general_ci
但是在插入数据时，中文显示为“？？”
通过在springboot application.properties中添加以下内容解决：
spring.datasource.url=jdbc:mysql://localhost:3306/poultryprodb?userSSL=false&useUnicode=true&characterEncoding=utf-8


###### 解决后端查询数据库正常，返回数据出现null
在实体类中使用驼峰命名，数据库字段使用下划线命名，通过@TableField注解解决。
```java
    @TableField(value = "user_name")
    private String userName;
```
如果使用MyBatis-Plus，可以在application.properties中添加配置：
```properties   
mybatis-plus.configuration.map-underscore-to-camel-case=true
```
多表查询比如
```
 //查询所有的进鸡记录，同时查询对应记录的用户。
    @Select("select * from purchase_record")
    @Results({
            @Result(column = "purchaseId", property = "purchaseId"),
            @Result(column = "price", property = "price"),
            @Result(column = "purchaseDate", property = "purchaseDate"),
            @Result(column = "buyer", property = "buyer"),
            @Result(column = "chickQuantity", property = "chickQuantity"),
            @Result(column = "chickType", property = "chickType"),
            @Result(column = "chickGender", property = "chickGender"),
            @Result(column = "userId", property = "user", javaType = User.class,
                one = @One(select = "com.example.poultrypro.mapper.UserMapper.findById")
            )
    })
    List<PurchaseRecord> selectAllPurchaseRecordAndUser();
```
里面最后的property里面的user是指根据userId查询到的User对象。
同时需要在purchaseRecord实体类中添加user属性。
```java
    @ApiModelProperty(value = "用户实体类", required = false, example = "")
    @TableField(exist = false)
    private User user;
```

总结：1.看数据库表中的字段和实体类中的字段是否一致。2.检查是否正确添加了get和set方法3.检查mapper里的对应关系


##前端开发

关闭Eslint
vue.config.js
```javascript
  lintOnSave: false,
```


通过后端配置CORS解决前端响应问题
在配置类中设置：
```
@Configuration
public class WebConfig implements WebMvcConfigurer {
}
```
```
 @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**")
                        .allowedOrigins("http://localhost:9528")  // 前端地址
                        .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                        .allowedHeaders("*")  // 允许所有请求头
                        .allowCredentials(true);
            }
        };
    }
```


###### 分页查询
1.定义一个配置类 MyBatisPlusConfig
```java
@Configuration
public class MyBatisPlusConfig {
    @Bean
    public MybatisPlusInterceptor paginationInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        PaginationInnerInterceptor paginationInnerInterceptor = new PaginationInnerInterceptor(DbType.MYSQL);
        return interceptor;
    }
}
```