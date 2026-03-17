---
layout: post
title: "MongoDB 联调快指南：接口结构、Mongoose 序列化和 Fly.io 环境变量这三个坑"
date: 2024-04-27 20:10:00 +0800
categories: [拆问题]
summary: "MongoDB 联调期最容易出问题的，通常不是查询语句本身，而是接口返回结构、`_id/__v` 序列化和部署环境变量未注入这三件事。"
pillars:
  - 拆问题
---

MongoDB 这段最容易让人误判的点是：  
你以为自己在“调数据库”，但实际卡住你的常常是接口约定和部署配置。

我把当时最有用的三件事收成这篇，方便以后快速复用。

## 1. 先把接口返回结构定死，前后端会省很多沟通

如果是前后端分离项目，建议尽早统一响应结构，不要每个接口临时发挥。

当时我后端返回里有一个典型结构：

```json
{
  "result": {
    "count": 13,
    "list": []
  }
}
```

关键不在字段名漂亮，而在约定稳定：

- `count` 负责总量
- `list` 负责列表数据
- 前端按固定位置取值

这一步做完后，分页、表格、图表都会轻松很多，因为数据入口一致了。

## 2. `__v` 和 `_id` 的处理，不要靠前端“临时抹平”

Mongoose 默认会返回 `_id` 和 `__v`。  
如果你希望前端拿到更干净的数据对象，最稳妥的是在模型层统一序列化：

```js
noteSchema.set('toJSON', {
  transform: (document, returnedObject) => {
    returnedObject.id = returnedObject._id.toString()
    delete returnedObject._id
    delete returnedObject.__v
  }
})
```

这个动作的价值在于：  
你不需要在每个前端页面再手动把 `_id` 改成 `id`，数据格式会在后端出口一次性收口。

## 3. Fly.io 上数据库连不上，先查环境变量而不是先怀疑代码

我当时部署后看到的错误核心是：

`mongoose.connect()` 的 URI 实际上是 `undefined`。

这意味着不是“MongoDB 挂了”，而是部署环境没拿到连接字符串。

在 Fly.io 上用 secrets 注入后就恢复了：

```shell
fly secrets set MONGODB_URI='mongodb://<username>:<password>@<host>:<port>/<database>'
```

这一步对排障非常关键：  
看到 `undefined` 先查配置注入，不要先改查询逻辑。

## 一个实用顺序：MongoDB 联调卡住时先按这 4 步看

1. 先确认接口返回结构是否统一（例如 `result.count` / `result.list`）。  
2. 再确认模型层是否做好 `toJSON` 转换（尤其 `id`、`_id`、`__v`）。  
3. 再看部署环境变量是否真实注入（本地 `.env` 和线上 secrets 是两套机制）。  
4. 最后才回头查查询语句本身。

## 这篇的核心结论

MongoDB 联调期最常见的失败点，很多时候不是数据库语法，而是：

- 接口结构没约定
- 数据对象没收口
- 环境变量没注入

把这三层先拉平，后面的数据开发会顺很多。
