---
layout: post
title:  "Spring AI Demo"
date:   2024-04-24 23:11:42 +0800
categories: jekyll update
---

Spring AI ��һ��ΪAI������Ƶ�Ӧ�ÿ�ܡ�����Ŀ���ǽ�Spring��̬ϵͳ���ԭ�������ֲ�Ժ�ģ�黯��ƣ�Ӧ����AI���򣬲��ƹ�ʹ��POJO��ΪӦ�ó���Ĺ����顣

> Spring AI is an application framework for AI engineering. Its goal is to apply to the AI domain Spring ecosystem design principles such as portability and modular design and promote using POJOs as the building blocks of an application to the AI domain.

ʹ��Spring initilizr����һ���µ�Spring Boot��Ŀ��
Java:21��SpringBoot��3.2.5�����Spring Web��Open AI��

������Ŀ�����ܻ��������ı���ԭ�����ڣ�spring ai������maven�Ĳֿ��У���Ҫ�ֶ���ӡ���pom.xml���������������
����
```shell
org.springframework.ai:spring-ai-spring-boot-autoconfigure:jar:0.8.1 failed to transfer from https://repo.spring.io/milestone during a previous attempt. Original error: Could not transfer artifact org.springframework.ai:spring-ai-spring-boot-autoconfigure:jar:0.8.1 from/to spring-milestones (https://repo.spring.io/milestone): repo.spring.io
```
pom.xml�����
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

  ��apikey ��ӵ�environment variables����Ϊ���ǽ���Ҫ���������в���


  String Templates

  augment context
  retrival augmentation generations��
  rag 


  JDK�޸�
  setting - commpiler - java compiler 
  project structure

