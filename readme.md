# cloudark

## 故事背景

从前有一群很懒的程序猿，生活在不为人知的云之暗面。

因为他们很懒，所以不愿意多写一个代码。

因为他们还有点良知，所以又不愿意糊弄，那些高并发啊，分布式啊，都要考虑进来。

他们又造了几个轮子：**cloudark 套件**

在这个轮子上，他们希望不用再写那些写了一辈子的重复代码了，

也不用再去关心程序性能问题了。【好吧，我要保证我不会在函数里写死循环！！】


## 从 0 开始创建一个微服务


### 1. 使用 cloudoll 创建 web 应用

创建一个目录：hello_world, 进去之后输入命令行 npm init。

引入 cloudoll 包

```
npm i cloudoll --save
```


创建一个入口文件 /index.js

```
require('cloudoll').KoaApplication();
```


创建文件 /api/open/hello.js

```
module.exports = {
  world: function *() {
    this.body = "你好世界。";
  }
};

```

现在启动服务：

```
node index.js
```

现在访问一下试试

http://localhost:3000/open/hello/world



### 2. 运行 cloudeer。

从 git 上 下载源码：

```
git clone https://code.aliyun.com/cloudark/cloudeer.git
```

如果 https 不好使，使用 http 。

进入到目录，进行 node 前戏工作 。

```
npm install
```

手工创建一个 /data 的目录用来存储数据。

运行：

```
node index.js
```

访问 http://localhost:8801/view

无需改动 cloudeer 任何代码。

打完收工!


### 3. 分布式的微服务 hello_world

好了，接下来，我们把第一步中写的程序变成可以被分布部署的微服务。

创建一个文件： /config/development.js

注意：在第一步创建的那个项目下哦，嫑搞错位置了。

内容如下：

```javascript
module.exports = {
  app_name      : "hello_world",
  debug         : true,
  port          : 3000,
  cloudeer      :{
    server: 'http://127.0.0.1:8801'
  },
  my_host       : "127.0.0.1"
};
```

其中 cloudeer 节点的配置会将这个应用变成分布式的微服务。


重启一下咯。

现在访问注册中心看看：

http://localhost:8801/view

和

http://localhost:8801/methods


好像很简单呀！

万里长征才走完第一步。

如果感兴趣请继续。

### 4 创建另一个微服务并调用 hello_world 微服务

实在太懒了，今天写不动了。


# cloudoll has more...

cloudoll 包含更多的功能，其中大多数功能都可以单独拿出来独立使用：

* [KoaApplication (上面的例子用的是这个)](./KoaApplication.md)

* [错误/例外处理](./Clouderr.md)

* [自动路由](./autoRouters.md)

* 数据的快捷访问 mongo mysql postgres

* 包含数据访问的服务层和控制层的基类（对应传统的MVC）

* schema 的自动验证

* 作为消费者调用远程其他微服务

* 权限验证（路由级别）

* 内网接口的防火墙功能（正在实现）


