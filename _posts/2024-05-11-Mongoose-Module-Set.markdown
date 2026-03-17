---
layout: post
title: "Mongoose 模块化改造：把“能跑”改成“能维护”"
date: 2024-05-11 17:32:42 +0800
categories: [拆问题]
cover: /assets/post-media/cover_mongoose_module_set.svg
summary: "Mongoose 从入口文件拆到 `models` 模块后，数据库连接、Schema、序列化规则的边界会立刻清晰，后续扩展模型也更稳。"
pillars:
  - 拆问题
---

Node 项目里最常见的混乱之一是：入口文件里塞满连接、模型、路由和配置。  
模块化 Mongoose 的目标不是“看起来规范”，而是降低维护风险。

## 推荐结构

```text
.
├── index.js
├── models/
│   └── note.js
├── .env
└── package.json
```

## 核心拆分点

1. `index.js` 只负责应用启动和中间件
2. `models/*.js` 负责连接、Schema 和 `toJSON` 规则
3. 配置统一从 `.env` 读取

## 关键代码（模型文件）

```js
const mongoose = require("mongoose")

const url = process.env.MONGODB_URI
mongoose.set("strictQuery", false)
mongoose.connect(url)

const noteSchema = new mongoose.Schema({
  content: String,
  important: Boolean,
})

noteSchema.set("toJSON", {
  transform: (_, returnedObject) => {
    returnedObject.id = returnedObject._id.toString()
    delete returnedObject._id
    delete returnedObject.__v
  },
})

module.exports = mongoose.model("Note", noteSchema)
```

## 上线注意点

- 本地用 `.env`
- 生产环境用平台密钥（例如 `fly secrets set`）
- `.env` 必须加入 `.gitignore`

## 一句话结论

Mongoose 模块化不是形式主义。  
它让入口更轻、模型更稳、故障定位更快，是 Node 后端从练习项目走向长期维护的关键一步。
