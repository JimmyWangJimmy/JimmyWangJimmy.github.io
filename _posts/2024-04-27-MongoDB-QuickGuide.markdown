
collections ��Ҫʱ������ʽ��Сд��

����һ��ǰ��˷�����Ŀ�Ľӿڹ淶��
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

������һ�����ǰ��ҳ��󣬺�˷��ص����ݣ�
{
  "result": {
    "count": 13,
    "list": [
      {
        "academic_field_id": 13,
        "update_time": 1705231116000,
        "academic_field": "��������ѧ",
        "create_time": 1705231116000
      },
      {
        "academic_field_id": 1,
        "update_time": 1710262802000,
        "academic_field": "������+",
        "create_time": 1704960940000
      },
      {
        "academic_field_id": 2,
        "update_time": 1710262813000,
        "academic_field": "�ֵ�����ѧ",
        "create_time": 1704960940000
      },
      {
        "academic_field_id": 3,
        "update_time": 1710262827000,
        "academic_field": "������ۺ�Ӧ��",
        "create_time": 1704960940000
      },
      {
        "academic_field_id": 4,
        "update_time": 1704960940000,
        "academic_field": "ѧ������4",
        "create_time": 1704960940000
      },
      {
        "academic_field_id": 5,
        "update_time": 1704960940000,
        "academic_field": "ѧ������5",
        "create_time": 1704960940000
      },
      {
        "academic_field_id": 6,
        "update_time": 1704960940000,
        "academic_field": "ѧ������6",
        "create_time": 1704960940000
      }
    ]
  }
}
���Կ����ӿڶ����ǣ�
{
    "result": {
        "count": 13,
        "list": []
    }
}
���У�result �ǹ̶��ģ�count ��������list �������б�count��ʾ��ѯ��������������list��ʾ��ѯ���������б�

{
    "result": {
        "count": 13,
        "list": [
            {
                "academic_field_id": 13,
                "update_time": 1705231116000,
                "academic_field": "��������ѧ",
                "create_time": 1705231116000
            },
            {
                "academic_field_id": 1,
                "update_time": 1710262802000,
                "academic_field": "������+",
                "create_time": 1704960940000
            },
            {
                "academic_field_id": 2,
                "update_time": 1710262813000,
                "academic_field": "�ֵ�����ѧ",
                "create_time": 1704960940000
            },
            {
                "academic_field_id": 3,
                "update_time": 1710262827000,
                "academic_field": "������ۺ�Ӧ��",
                "create_time": 1704960940000
            },
            {
                "academic_field_id": 4,
                "update_time": 1704960940000,
                "academic_field": "ѧ������4",
                "create_time": 1704960940000
            },
            {
                "academic_field_id": 5,
                "update_time": 1704960940000,
                "academic_field": "ѧ������5",
                "create_time": 1704960940000
            },
            {
                "academic_field_id": 6,
                "update_time": 1704960940000,
                "academic_field": "ѧ������6",
                "create_time": 1704960940000
            }
        ]
    }
}


���mongoose����ǰ��`__v`��`_id`����
��mongo.js�ļ���������Ĵ��룬���ǲ�û�гɹ���
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