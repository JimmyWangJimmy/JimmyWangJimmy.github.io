---
layout: post
title:  "Spring AI Demo"
date:   2024-04-24 23:11:42 +0800
categories: AI Java Spring
cover: /_posts/img/cover_spring_ai_engineering.svg
summary: 一次很早期的 Spring AI 上手记录，重点不在概念，而在依赖仓库、版本和接入方式这些真正会卡住启动的地方。
pillars:
  - 与AI共生
---
我第一次接 Spring AI 的时候，最大的感受不是“AI 已经能进 Spring 生态了”，而是：

这类项目看起来像 demo，实际最先卡人的还是工程接入。

也就是说，真正耗时间的往往不是“写一个调用模型的接口”，而是版本、仓库、依赖和运行方式这些很基础的东西。AI 一旦进入后端工程，第一道门并不是 prompt，而是构建系统。

## Spring AI 吸引人的地方是什么

Spring AI 当时最吸引我的，不只是它能调模型，而是它试图把 AI 能力放回 Spring 熟悉的工程语境里。

这意味着你不用把整个项目写成一套完全陌生的实验性结构，而可以继续沿用自己熟悉的后端组织方式：

- 配置驱动
- 依赖注入
- 统一的应用结构
- 更贴近 Java 工程化的接入方式

对一个原本就站在 Spring 世界里的开发者来说，这比“再去单独学一套 AI demo 脚手架”要自然得多。

## 真正第一个卡点：依赖拉不下来

我最早遇到的核心报错不是业务代码，而是构建期直接失败：

```shell
org.springframework.ai:spring-ai-spring-boot-autoconfigure:jar:0.8.1 failed to transfer from https://repo.spring.io/milestone during a previous attempt. Original error: Could not transfer artifact org.springframework.ai:spring-ai-spring-boot-autoconfigure:jar:0.8.1 from/to spring-milestones (https://repo.spring.io/milestone): repo.spring.io
```

这个问题很有代表性。因为它提醒了一件事：

很多 AI 框架在早期阶段，并不一定已经完全进入默认的 Maven 仓库链路。你以为“依赖坐标写对了就行”，但实际上构建系统根本找不到它。

所以第一步不是继续改业务代码，而是先把仓库源补齐。

## 解决方式：显式加入 Spring 的 milestone / snapshot 仓库

当时能跑起来的关键，是在 `pom.xml` 里显式补上这些 repository：

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

这一步很普通，但特别能说明问题：

AI 工程的第一层阻力，经常不是模型不够强，而是工程接入方式还没完全稳定。

## 第二个容易被忽略的点：密钥不该写死在项目里

一旦 demo 能跑起来，很多人会立刻把 API key 塞进配置文件，只求先通。

但我当时就已经觉得，既然这类东西迟早会进真实环境，那最开始就应该按更接近生产的方式处理：

- 放进环境变量
- 不直接写进代码
- 不让本地 demo 的方便变成后面部署时的隐患

这也是我后来越来越在意的一点：AI 项目最危险的地方，往往就在“它看起来像 demo，所以大家习惯先随便写”。

## 第三个感受：AI 进入后端以后，重点会从“会不会调接口”转向“怎么组织上下文”

真正开始上手之后，很快就会发现，单纯把模型接口调通其实不是最关键的。

更重要的是后面这些问题：

- 上下文怎么组织
- prompt 和业务结构怎么衔接
- RAG 到底要不要上
- 模型调用结果怎么被后端消费

也就是说，AI 进入 Spring 以后，重点不是“会不会发请求”，而是“如何把模型能力嵌进一条原本属于后端系统的链路”。

这一步开始之后，工程问题就会重新出现，只不过表现形式变成了：

- 配置如何分层
- 模型能力如何抽象
- 输出如何结构化
- 未来如何替换模型提供方

## 现在回头看，这篇最有价值的地方不是演示，而是提醒

Spring AI 这篇旧稿对我现在最有价值的，不是它留下了一个能跑的 demo，而是它很早提醒了我一件事：

**AI 项目一旦进入真实工程环境，最先考验的不是模型，而是接入能力。**

包括：

- 仓库和依赖是否可解析
- Java / JDK / Spring Boot 版本是否对齐
- 密钥是否走正确的配置方式
- 模型调用能否被组织进现有系统结构

这些问题如果一开始不正视，后面功能写得越多，返工越大。

所以如果把这次 Spring AI 上手压成一句话，我会这样说：

让我真正意识到 AI 正在进入工程世界的，不是“它会回答问题”，而是“它已经开始要求我像处理正常后端依赖一样处理它了”。
