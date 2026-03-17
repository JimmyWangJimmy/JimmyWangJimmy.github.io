---
layout: post
title: "PoultryPro 全栈排障清单：Swagger、CORS、映射、上传、分页"
date: 2024-05-11 22:00:00 +0800
categories: [拆问题]
summary: "全栈联调期最耗时的不是新功能，而是基础摩擦。把 Swagger、CORS、字段映射、上传和分页按固定顺序收敛，可以大幅降低返工。"
pillars:
  - 拆问题
---

这篇记录的是我在 PoultryPro 里反复遇到的五类高频问题，以及一个可复用排查顺序。

## 1. Swagger 启动报错

Spring Boot 2.6+ 常见兼容问题可先加：

```properties
spring.mvc.pathmatch.matching-strategy=ant_path_matcher
```

## 2. 前后端跨域

优先后端统一配 CORS，不要在前端逐点 patch。

## 3. MyBatis-Plus 字段映射

字段命名不一致时，优先两种方式：

- `@TableField("user_name")`
- 或开启驼峰映射

## 4. 文件上传

先定规则再写页面：

- `multipart/form-data`
- 文件大小限制
- 静态资源访问路径

## 5. 分页与多表查询

固定返回结构，避免“每个列表一套格式”。

## 推荐排障顺序

1. 文档可访问（Swagger）
2. 请求可到达（CORS）
3. 数据可映射（字段）
4. 资源可上传（文件）
5. 列表可稳定（分页）

## 一句话结论

全栈项目先清基础摩擦，再扩业务功能，团队节奏会稳定很多。
