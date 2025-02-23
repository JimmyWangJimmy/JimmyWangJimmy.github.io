
collections 需要时复数形式，小写。

定义一个前后端分离项目的接口规范：
{
    "code": 200,
    "msg": "success",
    "data": {
        "list": [],
        "total": 0
    }
}
{
    "code": 500,
    "msg": "error",
    "data": null
}
{
    "code": 401,
    "msg": "unauthorized",
    "data": null
}
{
    "code": 403,
    "msg": "forbidden",
    "data": null
}
{
    "code": 404,
    "msg": "not found",
    "data": null
}
{
    "code": 405,
    "msg": "method not allowed",
    "data": null
}
{
    "code": 406,
    "msg": "not acceptable",
    "data": null
}
{
    "code": 408,
    "msg": "request timeout",
    "data": null
}
{
    "code": 409,
    "msg": "conflict",
    "data": null
}
{
    "code": 413,
    "msg": "payload too large",
    "data": null
}
{
    "code": 415,
    "msg": "unsupported media type",
    "data": null
}
{
    "code": 500,
    "msg": "internal server error",
    "data": null
}
{
    "code": 501,
    "msg": "not implemented",
    "data": null
}
{
    "code": 502,
    "msg": "bad gateway",
    "data": null
}
{
    "code": 503,
    "msg": "service unavailable",
    "data": null
}
{
    "code": 504,
    "msg": "gateway timeout",
    "data": null
}
{
    "code": 505,
    "msg": "http version not supported",
    "data": null
}

{
    "code": 200,
    "msg": "success",
    "data": {
        "list": [],
        "total": 0
    }
}

{
    "code": 200,
    "msg": "success",
    "data": {
        "list": [],
        "total": 0
    }
}

{
    "code": 200,
    "msg": "success",
    "data": {
        "list": [],
        "total": 0
    }
}

{
    "code": 200,
    "msg": "success",
    "data": {
        "list": [],
        "total": 0
    }
}

{
    "code": 200,
    "msg": "success",
    "data": {
        "list": [],
        "total": 0
    }
}

{
    "code": 200,
    "msg": "success",
    "data": {
        "list": [],
        "total": 0
    }
}

下面是一个点击前端页面后，后端返回的数据：
{
  "result": {
    "count": 13,
    "list": [
      {
        "academic_field_id": 13,
        "update_time": 1705231116000,
        "academic_field": "计算语言学",
        "create_time": 1705231116000
      },
      {
        "academic_field_id": 1,
        "update_time": 1710262802000,
        "academic_field": "互联网+",
        "create_time": 1704960940000
      },
      {
        "academic_field_id": 2,
        "update_time": 1710262813000,
        "academic_field": "现当代文学",
        "create_time": 1704960940000
      },
      {
        "academic_field_id": 3,
        "update_time": 1710262827000,
        "academic_field": "计算机综合应用",
        "create_time": 1704960940000
      },
      {
        "academic_field_id": 4,
        "update_time": 1704960940000,
        "academic_field": "学术领域4",
        "create_time": 1704960940000
      },
      {
        "academic_field_id": 5,
        "update_time": 1704960940000,
        "academic_field": "学术领域5",
        "create_time": 1704960940000
      },
      {
        "academic_field_id": 6,
        "update_time": 1704960940000,
        "academic_field": "学术领域6",
        "create_time": 1704960940000
      }
    ]
  }
}
可以看出接口定义是：
{
    "result": {
        "count": 13,
        "list": []
    }
}
其中，result 是固定的，count 是总数，list 是数据列表。count表示查询到的数据总数，list表示查询到的数据列表。

{
    "result": {
        "count": 13,
        "list": [
            {
                "academic_field_id": 13,
                "update_time": 1705231116000,
                "academic_field": "计算语言学",
                "create_time": 1705231116000
            },
            {
                "academic_field_id": 1,
                "update_time": 1710262802000,
                "academic_field": "互联网+",
                "create_time": 1704960940000
            },
            {
                "academic_field_id": 2,
                "update_time": 1710262813000,
                "academic_field": "现当代文学",
                "create_time": 1704960940000
            },
            {
                "academic_field_id": 3,
                "update_time": 1710262827000,
                "academic_field": "计算机综合应用",
                "create_time": 1704960940000
            },
            {
                "academic_field_id": 4,
                "update_time": 1704960940000,
                "academic_field": "学术领域4",
                "create_time": 1704960940000
            },
            {
                "academic_field_id": 5,
                "update_time": 1704960940000,
                "academic_field": "学术领域5",
                "create_time": 1704960940000
            },
            {
                "academic_field_id": 6,
                "update_time": 1704960940000,
                "academic_field": "学术领域6",
                "create_time": 1704960940000
            }
        ]
    }
}


解决mongoose返回前端`__v`和`_id`问题
在mongo.js文件中有下面的代码，但是并没有成功：
```js
//remove mongo versioning field __v
noteSchema.set('toJSON', {
  transform: (document, returnedObject) => {
    returnedObject.id = returnedObject._id.toString()
    delete returnedObject._id
    delete returnedObject.__v
  }
})
```

###### Fly.io部署后连接数据库失败解决

在fly.io部署nodejs应用时，连接数据库失败，报错如下：
```
2024-05-21T05:20:26.277 app[1852e32c4e6368] nrt [info] > node index.js

2024-05-21T05:20:26.769 app[1852e32c4e6368] nrt [info] connecting to undefined

2024-05-21T05:20:26.786 app[1852e32c4e6368] nrt [info] Server running on port 3000

2024-05-21T05:20:26.786 app[1852e32c4e6368] nrt [info] error connecting to MongoDB: The `uri` parameter to `openUri()` must be a string, got "undefined". Make sure the first parameter to `mongoose.connect()` or `mongoose.createConnection()` is a string.
```

说明数据库未设置
运行fly secrets set 命令解决
```
fly secrets set MONGODB_URI='mongodb://<username>:<password>@<host>:<port>/<database>'
```


###### Eslint

```
npm install eslint --save-dev
```

```
npm init @eslint/config@0.4.2
```

```
npm install --save-dev @stylistic/eslint-plugin-js
```
