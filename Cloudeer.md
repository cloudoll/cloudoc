# Cloudeer

Cloudeer 是远程服务调用(RPC)的客户端工具。

<!--
Cloudeer 包含 2 部分内容：

cloudeer 是一个项目，是微服务的注册中心。

cloudoll.Cloudeer 是客户端调用工具。

## cloudeer 项目

这是微服务注册中心，管理了微服务的服务器地址，方法。

使用 cloudark 套件开发的 Web 应用必须依赖此服务。

开源地址在这里：

https://code.aliyun.com/cloudark/cloudeer

直接 clone 之后， 运行:

```
npm i
```

```
node index.js
```

## cloudeer-server 项目

这个是微服务注册中心的长连接版本

开源地址在这里：

https://code.aliyun.com/cloudark/cloudeer-server

添加一个登录用户

```
 node add-user.js -u knock -p password
```

运行:

```
npm i
```

```
node index.js -p 2345
```

##  cloudoll 中的 Cloudeer


使用这个工具来定期下载注册中心的的服务配置文件，并可以实现远程接口的调用。

首先需要创建 Cloudeer 实例：

```
var cloudoll = require('cloudoll');

var cloudeer = new cloudoll.Cloudeer({
    cloudeerUri: 'http://127.0.0.1:8801',
    downInterval: 10,
    upInterval: 9,
    app_name: 'hello_world',
    myHost: '127.0.0.1',
    myPort: 3002
});

```

如果你的应用程序是一个消费者，那么需要启动下载服务：

```
cloudeer.downloadService();
```


如果你的应用程序是一个微服务提供者，那么需要启动注册服务：

```
cloudeer.registerService();
```

如果你的方法需要纳入权限验证，那么需要提交方法到注册中心：

```
cloudeer.registerMethods([
  {url: "/summary", name: '统计', method: "GET"},
  {url: "/pay-way", name: '支付方法', method: "GET", open: true},
  {url: "/pay-way/delete", name: '支付方法删除', method: "POST"},
  {url: "/pay-ways", name: '支付方法列表', method: "GET"},
  {url: "/cash-order", name: '订单编辑', method: "POST"},
  {url: "/cash-orders", name: '订单列表', method: "GET"},
  {url: "/cash-order/collect", name: '收款', method: "POST"},
  {url: "/cash-order/delete", name: '删订单', method: "POST"}
]);

```

-->


作为消费者，调用远程的方法：

```
var data = yield cloudeer.invokeCo('GET', hello_world', '/open/hello/world', {id: 10});
```

远程方法调用的 callback 版本

```
cloudeer.invoke('GET', hello_world', '/open/hello/world', {id: 10}, function(err, data){
    //处理数据
);
```

服务数据存储在 Cloudeer.config 中，
如果你想实时查看微服务列表，你可以简单的输出（应当在开发或者测试环境下使用）。

```
this.echo(require('cloudoll').Cloudeer.config);
```


##  cloudoll.KoaAppliction 中的 Cloudeer

使用 KoaAppliction 创建的应用程序，

如果在配置文件中配置了 cloudeer 节点，则可以很方便地使用 Cloudeer 。

可以在路由中直接调用 this 关键字调用：

this.getCloudeer 和 this.postCloudeer【返回的是 generator】。

示例：

```
module.exports = {
  world: function *() {
    var dist = yield this.getCloudeer("cloudarling", "/open/district/children");
    this.echo(dist);
  }
};
```

参见：

https://code.aliyun.com/cloudark/cloudoc/blob/master/KoaApplication.md

