##### 一对多存储

###### 关系

> 这个一对多的多是存储在同一个集合中的

- users 和 addresses 是集合名

1. 嵌入关系：直接将数组嵌入到文档中

```json
{
   "_id":ObjectId("52ffc33cd85242f436000001"),
   "contact": "987654321",
   "dob": "01-01-1991",
   "name": "Tom Benzamin",
   "address": [
      {
         "building": "22 A, Indiana Apt",
         "pincode": 123456,
         "city": "Los Angeles",
         "state": "California"
      },
      {
         "building": "170 A, Acropolis Apt",
         "pincode": 456789,
         "city": "Chicago",
         "state": "Illinois"
      }]
} 
```

**查询**

```shell
>db.users.findOne({"name":"Tom Benzamin"},{"address":1})
```

2. 引用关系：亦然是将数组嵌入到父类文档中，但只存储Id

```json
{
   "_id":ObjectId("52ffc33cd85242f436000001"),
   "contact": "987654321",
   "dob": "01-01-1991",
   "name": "Tom Benzamin",
   "address_ids": [
      ObjectId("52ffc4a5d85242602e000000"),
      ObjectId("52ffc4a5d85242602e000001")
   ]
}
```

**引用关系的查询需要进行两次查询**

```shell
>var result = db.users.findOne({"name":"Tom Benzamin"},{"address_ids":1})
>var addresses = db.address.find({"_id":{"$in":result["address_ids"]}})
```

###### 数据库引用（DBRefs）

> 这个一对多的多是存储在不同的集合中的

DBRefs的三个参数

- `$ref`：集合名称
- `$id`：引用的Id
- `$db`：数据库名称，可选参数

插入指令

```shell
db.tzc.insert({"title": "测试文档3", 
"test":[DBRef("test", ObjectId("5ff13167addd124d5c95f3d7")), 
    DBRef("test", ObjectId("5ff13167addd124d5c95f3da"))]
    })
```

结果

```json
{
    "_id" : ObjectId("61a0908bfc4612ad53dd6395"),
    "title" : "测试文档3",
    "test" : [ /* 字段名 */
        {
            "$ref" : "test", /* DBRef 的集合名 */
            "$id" : ObjectId("5ff13167addd124d5c95f3d7")
        }, 
        {
            "$ref" : "test",
            "$id" : ObjectId("5ff13167addd124d5c95f3da")
        }
    ]
}
```

查询指令

```xhell
db.tzc.findOne({"title":"测试文档3"}).test[0].fetch()

var tzcResult = db.tzc.findOne({"title": "测试文档3"})
var testRef = tzcResult.test
testRef
```
