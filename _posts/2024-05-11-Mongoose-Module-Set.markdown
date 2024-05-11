---
layout: post
title:  "Mongoose Module Set"
date:   2024-05-11 17:32:42 +0800
categories: jekyll update
---

在根目录下创建一个`models`文件夹，并在其下创建一个名为`note.js`的文件，用来保存与mongoose相关的内容。

如下：
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

1.导入`dotenv`，用以管理环境变量。
```
npm install dotenv
```

2.在根目录下创建一个`.env`文件，用来保存环境信息。比如设置端口号：`PORT=3001`，设置`MONGODB_URI=...`。
可以在`.gitignore`文件中添加`.env`，这样在提交代码时就不会提交`.env`文件。

在index.js中引入
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
3.
之后由于项目是托管在fly.io上，所以输入
fly secrets set MONGODB_URI=... 来设置环境变量。