# orm.mongo

封装了 postgres，让他访问起来更加简单。


## 使用方法

连接数据库：

```javascript
var postgres = require('cloudoll').orm.postgres;

//这个方法只需在应用启动的时候调用一次
postgres.constr = "postgres://user:password@127.0.0.1:5432/db_name";
```

查询：

```javascript
var res = yield postgres.take("address", {
    where : "account_id=$accountId",
    params: {accountId: 10}
});

```

## API 列表

