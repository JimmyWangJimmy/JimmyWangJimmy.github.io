---
layout: post
title:  "Mongoose Module Set"
date:   2024-05-11 17:32:42 +0800
categories: jekyll update
---

�ڸ�Ŀ¼�´���һ��`models`�ļ��У��������´���һ����Ϊ`note.js`���ļ�������������mongoose��ص����ݡ�

���£�
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

1.����`dotenv`�����Թ�����������
```
npm install dotenv
```

2.�ڸ�Ŀ¼�´���һ��`.env`�ļ����������滷����Ϣ���������ö˿ںţ�`PORT=3001`������`MONGODB_URI=...`��
������`.gitignore`�ļ������`.env`���������ύ����ʱ�Ͳ����ύ`.env`�ļ���

��index.js������
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
֮��������Ŀ���й���fly.io�ϣ���������
fly secrets set MONGODB_URI=... �����û���������