
SpringBoot + MybatisPlus 后端程序开发
Web前端：Vue + ElementUI
### 第一讲：
B/S架构：前后端分离
优点：分散性高、维护方便、开发简单、共享性高、总拥有成本低

浏览器向Web服务器发送请求，Web服务器向数据库发送请求，数据库返回数据给Web服务器，Web服务器返回数据给浏览器。

工具介绍：IDEA
构建工具Maven。项目管理工具，对Java项目进行自动化构建和依赖管理。
作用：项目构建、依赖管理、统一开发结构。

### 第二讲：SpringBoot快速上手
SpringBoot是Spring的一个项目，简化了Spring的开发流程，提供了一些默认配置，开箱即用。
SSM：Spring + SpringMVC + Mybatis
SpringBoot：简化了SSM的开发流程。
- 遵循“约定优于配置”的原则
- 内嵌Tomcat、Jetty等服务器，无需部署WAR包。
- 提供定制化启动器starter，简化Maven配置，开箱即用
- 纯Java配置，没有XML配置文件
- 提供生产级的服务监控方案，如安全监控、性能监控、应用监控等

开发环境热部署
- SpringBoot-devtools， 设置热部署
1.加入依赖：
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

2.application.properties中加入：

```properties
spring.devtools.restart.enabled=true
spring.devtools.restart.additional-paths=src/main/java
```

3.设置中选中Build Project automatically -> apply -> OK
4.ctrl + shift + alt + / -> Registry -> compiler.automake.allow.when.app.running

### 第三讲：SpringBootController Web入门+路由映射+参数传递+数据响应
spring-boot-starter-web启动器：包括web、webMVC、json、tomcat等基础依赖组件，提供Web开发场景所需的所有底层依赖。
webMVC：SpringMVC的简化版，提供了一套基于注解的Web开发模式。
json: JSON数据解析组件
tomcat：内嵌Tomcat服务器

控制器：Controller
- @Controller：标识一个类是SpringMVC的Controller，返回的数据是页面和数据
- @RestController：标识一个类是SpringMVC的Controller，返回数据是JSON格式

现在使用前后端分离的开发模式，所以使用@RestController，仅返回数据

#### 路由映射
- @RequestMapping：主要负责URL路由映射
属性：value(URL路径)、method、params、headers

Method属性：限定请求方式 GET、POST、PUT、DELETE
注解：@GetMapping、@PostMapping、@PutMapping、@DeleteMapping


#### 参数传递
- @RequestParam：参数映射。将请求参数绑定到控制器的方法参数上，接收的参数来自HTTP请求体，或url的QueryString。
- @PathVariable：用来处理动态的URL，URL值作为控制器中处理方法的参数。
- @RequestBody：接收的参数是来自requestBody中，即请求体。

#### 数据响应

接口调试工具：
- apipost 
- postman

### 第四讲：SpringBoot文件上传+拦截器
完成了基础学习，开始进阶学习。

静态资源访问
- 定义过滤规则和静态资源位置
```
spring.mvc.static-path-pattern=/static/**
spring.resources.static-locations=classpath:/static/
```

文件上传原理
- 上传文件的本质是将文件的二进制流传输到服务器
表单的enctype属性规定在发送到服务器之前应该如何对表单数据进行编码。
当表单的enctype="application/x-www-form-urlencoded"（默认）时，form表单中的数据格式为键值对: key1=value1&key2=value2
当表单的enctype="multipart/form-data"时，form表单中的数据格式为二进制流数据

SpringBoot实现文件上传功能
修改文件上传大小限制
```properties
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=10MB
```


