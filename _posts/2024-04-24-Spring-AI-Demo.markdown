---
layout: post
title:  "Spring AI Demo"
date:   2024-04-24 23:11:42 +0800
categories: jekyll update
---

Spring AI 是一个为AI工程设计的应用框架。它的目标是将Spring生态系统设计原则（如可移植性和模块化设计）应用于AI领域，并推广使用POJO作为应用程序的构建块。

> Spring AI is an application framework for AI engineering. Its goal is to apply to the AI domain Spring ecosystem design principles such as portability and modular design and promote using POJOs as the building blocks of an application to the AI domain.

使用Spring initilizr创建一个新的Spring Boot项目。
Java:21，SpringBoot：3.2.5，添加Spring Web和Open AI。

启动项目，可能会出现下面的报错，原因在于，spring ai还不在maven的仓库中，需要手动添加。在pom.xml中添加如下依赖：
报错：
```shell
org.springframework.ai:spring-ai-spring-boot-autoconfigure:jar:0.8.1 failed to transfer from https://repo.spring.io/milestone during a previous attempt. Original error: Could not transfer artifact org.springframework.ai:spring-ai-spring-boot-autoconfigure:jar:0.8.1 from/to spring-milestones (https://repo.spring.io/milestone): repo.spring.io
```
pom.xml中添加
```xml
  <repositories>
    <repository>
      <id>spring-milestones</id>
      <name>Spring Milestones</name>
      <url>https://repo.spring.io/milestone</url>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </repository>
    <repository>
      <id>spring-snapshots</id>
      <name>Spring Snapshots</name>
      <url>https://repo.spring.io/snapshot</url>
      <releases>
        <enabled>false</enabled>
      </releases>
    </repository>
  </repositories>
  ```

  将apikey 添加到environment variables，因为我们将来要生产环境中部署。


  String Templates

  augment context
  retrival augmentation generations是
  rag 


  JDK修改
  setting - commpiler - java compiler 
  project structure

