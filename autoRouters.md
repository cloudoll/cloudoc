# autoRouters：koa 的自动路由

这部分是以 koa 的中间件提供，废话不说，先上代码：

```
var koa = require('koa');
var KoaMiddle = require('cloudoll').KoaMiddle;

var app = koa();

var koaMiddle = new KoaMiddle();
var router = koaMiddle.autoRouters();
app.use(router.routes());

```

上面的例子将自动映射 '/api/open', '/api/admin', '/api/inner'  这个三个文件夹的路由。

现在我们在 '/api/open' 文件夹下增加一个 hello.js 文件：

```
module.exports = {
  world: function *() {
    this.body = "你好世界。";
  },
  $worldOfCloudbeer: function *() {
    this.body = "你好, 啤酒云。";
  }
};

```

上面的文件「/api/open/hello.js 」将映射出两个路由：

*GET /open/hello/world*

*POST /open/hello/world-of-cloudbeer*

请注意：路径里没有 api 这个单词。

方法名以 $ 开始的，将被映射成 POST 接口。

方法名的驼峰会被自动转化成短横杠连接。

目前仅支持 GET 和 POST 两类方法。

**请注意：使用 KoaApplication 无需手工调用以上的代码，只需要将商业逻辑放入相应的目录中**

## 为什么默认会使用三组路由？

 考虑到微服务一般会有三种应用场景：

 * 公网：/open
 * 管理后台：/admin
 * 内网调用（微服务之间调用）: /inner

在 KoaApplication 中，系统将会自动提交这三类方法到 cloudeer 注册中心以进行自动的权限绑定。

## 更改默认的扫描目录

使用构造函数修改扫描路径：

```javascript
var router = koaMiddle.autoRouters(['/controllers/open', '/controllers/manage']);
```

在 KoaApplication 应用中，修改配置文件即可

```
controller_dirs: ['/api/open', '/api/admin', '/api/inner'],
```

**请注意：修改默认扫描路径后，权限验证中间键将失效**

