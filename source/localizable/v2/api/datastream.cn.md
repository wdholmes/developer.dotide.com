---
layout: docs
category: api
section: datastream
toc: article
title: 数据流操作
---

## 罗列数据流

罗列数据库中一组特定的数据流，返回结果按照数据流 id 的字母顺序升序排列。

```
GET /:db/datastreams
```

#### 参数
| 名称        | 类型    | 说明 |
| ---------- | ------ | ------------------------------------------------------ |
| ids        | string | 数据流 id 的列表，不同的 id 以`,`分隔。id 在该列表中的数据流将被返回。 |
| tags       | string | 数据流标签的列表，不同的标签以`,`分隔。标签包含 `tags` 中所有标签的数据流将被返回。|
| limit      | number | 返回数据流的个数的最大值。**默认值**为`100`，**最大值**为`1000`。 |
| offset     | number | 返回数据流的偏移量，即返回结果中跳过开头的 `offset`个数据流，与 `limit` 配合以达到分页的效果。**默认值**为`0` |

如果 `ids` 和 `tags` 同时提供，则二者结果的“交集”为最终返回的数据流。

> 示例

```
/demo/datastreams?tags=a,b&limit=500
```

#### 响应

```
Status: 200 OK
```

```json
[
  {
    "id": "51e51544fa36a48592000074",
    "name": "Demo Datastream",
    "tags": ["a", "b", "c"],
    "properties": {
        "prop": 1
    },
    "datapoints_count": 0,
    "current_t": null,
    "current_v": null,
    "created_at": "2014-01-03T00:00:00.001+08:00",
    "updated_at": "2014-01-03T00:01:02.456+08:00"
  }
]
```


## 创建一条数据流

```
POST /:db/datastreams
```

#### 输入

| 名称        | 类型             | 说明 |
| ---------- | ---------------- | ------------------------------------------------------ |
| id         | string           | 指定需要创建的数据流的 id，如不输入，则由系统默认生成。**要求：当前数据库中唯一。** |
| name       | string           | 数据流的显示名称。 |
| tags       | array            | 标签列表。每个数据流可包含一组标签，每个标签为一个字符串。 |
| properties | hash             | 自定义属性。每个数据流可包含一组自定义的属性，每个属性是由`属性名: 属性值`构成的键值对。 |

> 示例

```json
{
  "id": "51e51544fa36a48592000074",
  "name": "demo",
  "tags": ["a", "b", "c"],
  "properties": {
      "meterial": "steel",
      "type": "number"
  }
}
```

#### 响应

```
Status: 201 Created
Location: https://api.dotide.com/v1/demo/datastreams/51e51544fa36a48592000074
```

```json
{
  "id": "51e51544fa36a48592000074",
  "name": "demo",
  "tags": ["a", "b", "c"],
  "properties": {
      "meterial": "steel",
      "type": "number"
  },
  "datapoints_count": 0,
  "current_t": null,
  "current_v": null,
  "created_at": "2014-01-03T00:00:00.001+08:00",
  "updated_at": "2014-01-03T00:01:02.456+08:00"
}
```


## 获取一条数据流

```
GET /:db/datastreams/:id
```

#### 响应

```
Status: 200 OK
```

```json
{
  "id": "51e51544fa36a48592000074",
  "name": "demo",
  "tags": ["a", "b", "c"],
  "properties": {
      "prop1": 1
  },
  "datapoints_count": 1,
  "current_t": "2014-01-03T00:01:02.123+08:00",
  "current_v": 100,
  "created_at": "2014-01-03T00:00:00.001+08:00",
  "updated_at": "2014-01-03T00:01:02.456+08:00"
}
```


## 更新一条数据流

```
PUT /:db/datastreams/:id
```

#### 输入

| 名称        | 类型             | 说明 |
| ---------- | ---------------- | ------------------------------------------------------ |
| name       | string           | 数据流的显示名称。 ｜
| tags       | array            | 标签列表。每个数据流可包含一组标签，每个标签为一个字符串。 |
| properties | hash             | 自定义属性。每个数据流可包含一组自定义的属性，每个属性是由`属性名: 属性值`构成的键值对。 |

> 示例

```json
{
  "name": "demo2",
  "tags": ["a", "b", "c", "d"],
  "properties": {
      "prop1": 2
  }
}
```

#### 响应

```
Status: 200 OK
```

```json
{
  "id": "51e51544fa36a48592000074",
  "name": "demo2",
  "tags": ["a", "b", "c", "d"],
  "properties": {
      "prop1": 2
  },
  "datapoints_count": 1,
  "current_t": "2014-01-03T00:01:02.123+08:00",
  "current_v": 100,
  "created_at": "2014-01-03T00:00:00.001+08:00",
  "updated_at": "2014-01-03T00:02:03.567+08:00"
}
```


## 删除一条数据流

```
DELETE /:db/datastreams/:id
```

#### 响应

```
Status: 204 No Content
```

[auth]: /v2/auth/overview.html