# orm.mongo

封装了 mongodb，让他访问起来更加简单。

基本和原始的参数保持了一致。

请参考这个链接：

[Node.js MongoDB Driver API](http://mongodb.github.io/node-mongodb-native/2.0/api/)


## 使用方法

连接数据库：

```javascript
var mongo = require('cloudoll').orm.mongo;

//这个方法只需在应用启动的时候调用一次
mongo.connect("mongodb://user:password@127.0.0.1:27017/order");
```

查询：

```javascript
var res = yield mongo.findOne('collectionName', {_id: ObjectId('xxxxx')});

```

## API 列表

以下的 api 中， collectionName 表示的是集合名称。

其他的参数和原驱动保持了一致。

### connect

连接数据库，这个方法一般在你的 web 应用启动的时候执行一次就可以了。

```
function (dbUrl)
```

### find

查询文档列表

```
function *(collectionName, query, options)
```

### count

数数

```
function *(collectionName, query, options)
```

### list

查询文档列表，这个和 find 的区别是，他会返回出满足查询条件的总数，为了翻页而生。

```
function *(collectionName, query, options)
```

### findOne

找到一个文档

```
function *(collectionName, query, options)
```

### exists

判断是否存在满足条件的文档

```
function *(collectionName, query)
```

### deleteOne

删除一个文档

```
function *(collectionName, query, options)
```

### delete

删除满足条件的所有文档

```
function *(collectionName, query, options)
```

### insert

插入文档，此方法可以支持插入多个文档，当 docs 是一个文档数组的时候。

```
function *(collectionName, docs, query, options)
```

### updateOne

更新一个文档

```
function *(collectionName, doc, query, options)
```

### findOneAndIncWithLock

找到一个记录并且做自增操作

```
function *(collectionName, doc, query, options)
```

### findOneAndUpdate 这个好像没有



### updateById

指定 _id 更新

```
function *(collectionName, doc, _id, options)
```


### save ?? 这个目前有问题啊

如果有指定 _id 字段，则是 findOneAndUpdate 操作, 否则是 insert 操作

```
function *(collectionName, doc, query, options)
```

### sum

字段做 和 操作

```
function *(collectionName, key, match)
```