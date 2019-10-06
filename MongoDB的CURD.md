

## 创建、更新和删除文档

创建
`db.collection.insertOne()`  
`db.collection.insertMany()` 
读取
`db.collection.find()`
更新
`db.collection.updateOne()`
`db.collection.updateMany()`
`db.collection.replaceOne()`
删除
`db.collection.deleteOne()`
`db.collection.deleteMany()`



### 创建/插入

向指定集合中插入一条/多条文档数据
`db.collection.insert()`
向指定集合中插入一条文档数据
`db.collection.insertOne()`
insertOne()返回包含新插入文档的_id字段值的文档

向指定集合中插入多条文档数据
`db.collection.insertMany()`

以下方法还可以向集合中添加新文档：

- db.collection.update() 与选项一起使用时。upsert: true
- db.collection.updateOne() 与选项一起使用时。upsert: true
- db.collection.updateMany() 与选项一起使用时。upsert: true
- db.collection.findAndModify() 与选项一起使用时。upsert: true
- db.collection.findOneAndUpdate() 与选项一起使用时。upsert: true
- db.collection.findOneAndReplace() 与选项一起使用时。upsert: true
- db.collection.save()。
- db.collection.bulkWrite()。

### 查询

**查询集合中所有文档**

`db.collection.find({})`

相当于SQL语句中的

`SELECT * FROM collection`

**指定查询条件**
`db.collection.find({status: "D"})`
相当于SQL语句中的

```sql
SELECT * FROM collection WHERE status = "D"
```

#### 查询运算符

**指定In条件**
`{<field>: {<operator1>:<value1>}}`

从collection 集合中检索所有文档，其中status等于"A"或"D"：
`db.collection.find( { status: { $in: [ "A", "D" ] } } )`
相当于SQL语句中的

```sql
SELECT * FROM collection WHERE status in ("A", "D")
```

对同一字段执行相等性检查时，请使用`$in`运算符而不是`$or`

**指定And条件**

检索collection 集合中statusequals "A" 和 qty小于（$lt）的所有文档
`db.collection.find( { status: "A", qty: { $lt: 30 } } )`

相当于SQL语句中的

```sql
SELECT * FROM collection WHERE status = "A" AND qty < 30
```

**指定Or条件**
检索collection 集合中statusequals "A" 或 qty小于（$lt）的所有文档
`ddb.collection.find( { $or: [ { status: "A" }, { qty: { $lt: 30 } } ] } )`

相当于SQL语句中的

```sql
SELECT * FROM collection WHERE status = "A" OR qty < 30
```

**指定And以及Or**

```js
db.collection.find( {
     status: "A",
     $or: [ { qty: { $lt: 30 } }, { item: /^p/ } ]
} )
```

相当于SQL语句中的

```sql
SELECT * FROM collection WHERE status = "A" AND (qty < 30 OR item LIKE "p%")
```

#### 查询嵌套文档

实例文档

```json
{ item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
{ item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "A" },
{ item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
{ item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
{ item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" }
```

**匹配嵌套文档**

`db.collection.find({size: {h: 14, w: 21, uom: "cm"}})`

嵌套文档的等式匹配需要指定文档的完全匹配，包括字段顺序

**查询嵌套字段**

使用点表示法在嵌套文档的字段上指定查询条件
`field.nestedField`

选择uom 嵌套在size字段中的字段等于"in"的所有文档
`db.collection.find({"size.uom": "in"})`

使用运算符查询嵌套字段

`db.collection.find({"size.h": {$lt: 15}})`

查询选择嵌套字段h小于15，嵌套字段uom等于"in"，并且status字段等于"D"的所有文档

```
db.collection.find(
  {
      "size.h": {$lt:15},
      "size.uom": "in",
      "status": "D"
  }
)
```

#### 查询数组

```js
db.collection.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], dim_cm: [ 14, 21 ] },
   { item: "notebook", qty: 50, tags: ["red", "blank"], dim_cm: [ 14, 21 ] },
   { item: "paper", qty: 100, tags: ["red", "blank", "plain"], dim_cm: [ 14, 21 ] },
   { item: "planner", qty: 75, tags: ["blank", "red"], dim_cm: [ 22.85, 30 ] },
   { item: "postcard", qty: 45, tags: ["blue"], dim_cm: [ 10, 15.25 ] }
]);
```

**匹配数组**
使用查询文档`{<field>:<value>}`匹配确切的数组，包括元素顺序
`db.collection.find( { tags: ["red", "blank"]} )`

不考虑数组顺序或其他元素，使用`$all`
`db.collection.find({tags: {"$all": ["red", "blank"] }})`

查询数组字段是否包含至少一个具有指定值的元素
`db.collection.find( { tags: "red" } )`

查询数组dim_cm包含至少一个值大于25的元素的所有文档 
`db.collection.find( { dim_cm: {$gt: 25} } )`

指定多个条件

**查询具有复合过滤条件的数组**

一个元素可以满足大于15而另一个元素可以满足小于20，或者单个元素可以满足两个

```
db.collection.find(
    {
        dic_cm: {
            $gt: 15,
            $lt: 20
        }
    }
)
```

**查询符合多个条件的数据组元素**
使用`$elemMatch`运算符在数组的元素上指定多个条件，至少一个数组元素满足所有指定的条件

```js
db.collection.find(
    dim_cm: {
    $elemMatch: {
            $gt: 22,
            $lt: 30
        }
    }
)
```

**通过数组索引位置查询元素**
使用点表示法，可以为数组的特定索引或位置处的元素指定查询条件

查询数组dim_cm中第二个元素大于25的所有文档
`db.collection.find({ "dim_cm.1": { $gt: 25 } })`

**按数组长度查询数组**
使用`$size`运算符按元素个数查询数组

查询数组tags有三个元素的文档
`db.collection.find( { "tags": { $size: 3 } } )`

#### 查询嵌入式文档数组

```js
db.collection.insertMany( [
   { item: "journal", instock: [ { warehouse: "A", qty: 5 }, { warehouse: "C", qty: 15 } ] },
   { item: "notebook", instock: [ { warehouse: "C", qty: 5 } ] },
   { item: "paper", instock: [ { warehouse: "A", qty: 60 }, { warehouse: "B", qty: 15 } ] },
   { item: "planner", instock: [ { warehouse: "A", qty: 40 }, { warehouse: "B", qty: 5 } ] },
   { item: "postcard", instock: [ { warehouse: "B", qty: 15 }, { warehouse: "C", qty: 35 } ] }
]);
```

**查询嵌套在数组中的文档**
嵌套文档上的等式匹配需要指定文档的*完全匹配*，包括字段顺序

`db.collection.find( { "instock": { warehouse:"A", qty: 5 } } )`

**在文档数组中嵌入的字段上指定查询条件**

查询instock数组中qty字段值小于或等于20的所有文档

`db.collection.find( { "instock.qty": { $lte: 20 } } )`

**使用数组索引查询嵌入文档中的字段**

查询instock数组第一个元素中qty字段小于等于20的所有文档
`db.collection.find( { "instock.0.qty": { $lte: 20 } } )`

**为文档数组指定多个条件**

使用`$elemMatch`运算符在嵌入文档数组上指定多个条件，以便至少一个嵌入文档满足所有指定条件

```js
db.inventory.find( { 
    "instock": { $elemMatch: { qty: 5, warehouse: "A" } } 
} )
db.inventory.find( { 
    "instock": { $elemMatch: { qty: { $gt: 10, $lte: 20 } } }
} )
```

**满足标准的元素组合**

如果数组字段上的复合查询条件不使用`$elemMatch`运算符，则查询将选择其数组包含满足条件的任何元素组合的那些文档

`db.inventory.find( { "instock.qty": { $gt: 10,  $lte: 20 } } )`

#### 查询返回的字段

默认情况下，MongoDB中的查询返回匹配文档中的所有字段。要限制MongoDB发送到应用程序的数据量，您可以包含projection文档以指定或限制要返回的字段

**返回指定字段**

只返回item，status，_id字段

```
db.collection.find(
    {status: "A"},
    {item: 1, status: 1}
)
```

对应的SQL语句为

```sql
SELECT _id, item, status from collection WHERE status = "A"
```

**抑制_id字段**
通过显示设置`_id:0`来删除返回中的_id字段

```
db.collection.find(
    {status: "A"},
    {item: 1, status: 1, _id: 0}
)
```

对应的SQL语句为

```sql
SELECT item, status from collection WHERE status = "A"
```

**反向排除**
返回除 匹配文档中status的instock字段和字段之外的所有字段

```
db.collection.find(
    {status: "A"},
    {instock: 0, status: 0}
)
```

**嵌入式文档返回特定字段**

```
db.collection.find(
    {status: "A"},
    {instock: 0, status: 0, "size.uom": 1}
)
```

**抑制嵌入式文档返回特定字段**

```
db.collection.find(
    {status: "A"},
    {"size.uom": 0}
)
```

**数组中嵌入文档的projection**
返回_id, item, status, 嵌入在instock数组中的qty字段

```
db.collection.find(
    {status: "A"},
    {item: 1, status:1, "instock.qty": 1}
)
```

**返回数组中的项目特定数组元素**
使用`$slice`运算符返回instock数组中的最后一个元素

```
db.collection.find(
    {status: "A"},
    {item: 1, status: 1, instock: {$slice: -1}}
)
```

### 删除文档

删除集合
`db.collection.drop()`
如删除集合下全部文档：
`db.collection.deleteMany({})`
删除 status 等于 A 的全部文档：
`db.collection.deleteMany({ status : "A" })`
删除 status 等于 D 的一个文档：
`db.collection.deleteOne( { status: "D" } )`



## 操作符

`$elemMatch`操作符匹配包含数组字段的文档，其中至少有一个元素匹配所有指定的查询条件
`$slice`操作符控制该查询返回的数组的项数
`db.collection.find({field: value}, {array: {$slice: countj}})`
`$`
`$push`