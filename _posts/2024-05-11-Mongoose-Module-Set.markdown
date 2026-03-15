---
layout: post
title: "把 Mongoose 单独拆成 models 模块之后，Node 项目会清爽很多"
date:   2024-05-11 17:32:42 +0800
categories: [拆问题]
cover: /assets/post-media/cover_mongoose_module_set.svg
summary: "Mongoose 真正值得单独拆模块，不是因为“教程都这么写”，而是数据库连接、Schema 和序列化逻辑一旦独立，项目结构会立刻清楚很多。"
pillars:
  - 拆问题
---

很多 Node 项目一开始都能跑，但代码会很快变得挤在一起：  
路由、数据库连接、Schema、环境变量全都混在 `index.js` 里。

Mongoose 单独拆成 `models` 模块，对我来说最直接的价值不是“更规范”，而是让项目结构终于开始像一个能扩展的服务。

## 第一步：把连接和模型放进单独文件

在根目录创建 `models` 文件夹，并在其中新建 `note.js`。

最基础的结构像这样：

```js
const mongoose = require('mongoose')

mongoose.set('strictQuery', false)


const url = process.env.MONGODB_URI


console.log('connecting to', url)

mongoose.connect(url)

  .then(result => {
    console.log('connected to MongoDB')
  })
  .catch(error => {
    console.log('error connecting to MongoDB:', error.message)
  })

const noteSchema = new mongoose.Schema({
  content: String,
  important: Boolean,
})

noteSchema.set('toJSON', {
  transform: (document, returnedObject) => {
    returnedObject.id = returnedObject._id.toString()
    delete returnedObject._id
    delete returnedObject.__v
  }
})


module.exports = mongoose.model('Note', noteSchema)
```

这一步做完之后，数据库连接、Schema 定义和 JSON 序列化规则都从主入口文件里拆出来了。

## 第二步：把环境变量也一起正规化

这里最适合同时引入 `dotenv`：

```
npm install dotenv
```

然后在根目录创建 `.env` 文件，用来存放：

- `PORT=3001`
- `MONGODB_URI=...`

同时把 `.env` 放进 `.gitignore`，避免把真实连接信息提交出去。

主入口文件里只负责读取并启动：

```js
require('dotenv').config()
const express = require('express')
const app = express()

const Note = require('./models/note')

// ..


const PORT = process.env.PORT
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

## 第三步：部署时不要沿用本地写法

如果项目托管在 `fly.io` 这类平台，本地 `.env` 的思路就要切到平台密钥管理。

例如：

```shell
fly secrets set MONGODB_URI=...
```

这样你就不会把部署环境建立在本地文件存在与否上。

## 这件事真正改善了什么

把 Mongoose 单独拆模块之后，项目会立刻清楚很多：

- 入口文件只负责启动服务
- 数据模型集中管理
- 连接逻辑不再散落
- 序列化规则有固定位置
- 后面新增模型时也更容易复制同样结构

这种清晰感在项目小的时候不明显，但一旦接口和模型变多，它会直接决定你后面改代码时会不会越改越乱。

## 我现在怎么看这一步

现在回头看，这一步的价值不只是“学会了 Mongoose”，而是开始理解后端项目为什么需要分层。

真正重要的不是记住 `mongoose.model(...)` 怎么写，而是慢慢形成一个习惯：

连接归连接，模型归模型，启动归启动，配置归配置。

当这些东西开始各回各位，项目才会从“能跑”变成“能继续长”。
