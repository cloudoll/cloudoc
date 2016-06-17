# cloudark

## 故事背景

从前有一群很懒的程序猿，生活在不为人知的云之暗面。

因为他们很懒，所以不愿意多写一个代码。

因为他们还有点良知，所以又不愿意糊弄，那些高并发啊，分布式啊，都要考虑进来。

他们又造了几个轮子：**cloudark 套件**

在这个轮子上，他们希望不用再写那些写了一辈子的重复代码了，也不用再去关心新能问题了。

## cloudeer

这是微服务的注册服务器。

## cloudoll

包含了一系列工具，协助快速建立应用程序。

## cloudarling

这是一个微服务，提供 passport 服务，以及权限管理等操作。


## 从 0 开始创建一个微服务

### 运行 cloudeer

从 git 上 下载源码：

```
git clone https://code.aliyun.com/cloudark/cloudeer.git
```

进入到目录，进行 node 前戏工作。

```
npm install
```

手工创建一个 ./data 的目录用来存储数据

运行：

```
node index.js
```

访问 http://localhost:8801/view

无需改动 cloudeer 任何代码。

打完收工!

先让这个服务运行着，下面的步骤中还需要依赖这个服务。


### 使用 cloudoll 创建一个微服务

创建一个 node 应用程序 （怎么创建？自己去研究咯）。

引入 cloudoll 包

```
npm i cloudoll --save
```

创建一个文件： /config/development.js 内容如下：

```javascript
module.exports = {
  debug         : true,
  port          : 22221,
  cloudeer_url  : "http://127.0.0.1:8801",
  my_ip         : "127.0.0.1",
  controller_dirs: ['/api/open', '/api/admin'],
  schema_path    : './schema',
  my_errors_path  : './my-errors.js'
};
```

创建一个入口文件 index.js

```
var KoaApp = require('cloudoll').KoaApplication;
var app    = KoaApp();
var router = app.router;
var cloudeer = app.cloudeer;
```

上面的两个变量  router 和 cloudeer 留着待用。

创建文件 /api/open/hello.js

```
module.exports = {
  world: function () {
    this.body = "你好世界。";
  }
};

```

请使 cloudeer 服务运行先。

现在启动服务：

```
node index.js
```


现在访问一下试试

http://localhost:22221/open/hello/world

稍等一下访问

http://localhost:8801/view

和

http://localhost:8801/methods

试试。

好像很简单吧！

万里长征才走完第一步。

# cloudoll has more...

cloudoll 包含更多的功能，其中大多数功能都可以单独拿出来独立使用：

* [内置的 KoaApplication (上面的例子用的是这个)](./KoaApplication.md)

* [错误/例外处理](./Clouderr.md)

* [自动路由](./autoRouters.md)

* 数据的快捷访问 mongo mysql postgres

* 包含数据访问的服务层和控制层的基类（对应传统的MVC）

* schema 的自动验证

* 作为消费者调用远程其他微服务

* 权限验证（路由级别）

* 内网接口的防火墙功能（正在实现）


