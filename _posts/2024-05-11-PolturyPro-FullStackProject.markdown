

开发流程：
1.明确需求，设计数据库表结构
2.开发后端接口
3.进行前端UI设计
4.开发前端
5.测试
6.交由使用人员使用
7.完成项目文档，提交申请软著专利。

目前进行：2.开发后端接口


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
