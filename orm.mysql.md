# orm.mysql

封装了 mysql，让他访问起来更加简单。

## 使用方法

连接数据库：

```javascript
var mysql = require('cloudoll').orm.mysql;

//这个方法只需在应用启动的时候调用一次
mysql.connect({
  connectionLimit: 10,
  host           : '127.0.0.1',
  user           : 'user',
  password       : 'passworo',
  database       : 'test'
});
```

查询：

```javascript
var res = mysql.list('a_table', {
    where : "id>?",
    params: [10],
    limit : 1000
});

```

## API 列表