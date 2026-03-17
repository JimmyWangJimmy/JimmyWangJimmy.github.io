---
layout: post
title: "PoultryPro 全栈项目排障记录：从 Swagger 到 CORS，再到 MyBatis-Plus 字段映射"
date: 2024-05-11 22:00:00 +0800
categories: [拆问题]
summary: "全栈项目推进期最耗时的常常不是写功能，而是把文档、跨域、字段映射、上传和分页这些基础环节逐个压平。"
pillars:
  - 拆问题
---

PoultryPro 这段开发里，我最深的感受是：  
功能开发本身并不是最慢的，最慢的是把各种“基础摩擦”逐个清掉。

这篇整理的是当时最实用的几个排障点。

## 1. SpringBoot 2.7 + Swagger 报错，先看路径匹配策略

典型错误：

`Failed to start bean 'documentationPluginsBootstrapper' ... NullPointerException`

在 SpringBoot 2.6+ 之后，路径匹配策略变动会触发这类兼容问题。  
我当时通过下面配置恢复：

```properties
spring.mvc.pathmatch.matching-strategy=ant_path_matcher
```

然后再访问：

`http://localhost:8888/swagger-ui/index.html`

## 2. 前后端跨域不通，不要在前端层反复猜

这类问题最稳的做法是后端统一配置 CORS，而不是每个前端请求点做临时 patch。

核心思路是：

- 明确允许来源（开发期例如 `localhost:9528`）
- 明确允许方法
- 明确允许请求头和凭证

一旦后端这层统一了，前端联调会顺很多。

## 3. MyBatis-Plus 字段映射是高频坑

“数据库查得出来，但返回给前端是 null”，这类问题在全栈项目里非常常见。

我当时主要用两种方式收口：

- 用注解显式映射字段，例如 `@TableField("user_name")`
- 或开启驼峰映射配置：

```properties
mybatis-plus.configuration.map-underscore-to-camel-case=true
```

结论是：  
字段命名策略不统一时，问题几乎必然出现；尽早统一就能省大量时间。

## 4. 文件上传和静态资源要提前定规则

文件上传这块如果不提前定规则，中后期很容易返工。  
我当时提前固定了：

- 请求格式走 `multipart/form-data`
- 上传大小限制
- 静态资源路径映射

这些看起来像“配置细节”，但它们直接决定了接口稳定性和前端可用性。

## 5. 分页和多表查询：要么规范，要么混乱

全栈项目后期常见问题是“列表能查，但结构越来越乱”。  
最重要的不是 SQL 花活，而是返回结构和实体关系定义一致。

特别是多表查询时，实体里的附加对象要显式声明为非表字段，否则很容易出现奇怪映射问题。

## 一句话结论

PoultryPro 这段最有价值的经验是：  
全栈项目要先把基础摩擦清掉，再谈功能规模。  
Swagger、CORS、字段映射、上传、分页这些基础环节每提前一天稳定，后面都会轻松很多。
