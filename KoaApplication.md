# KoaApplication

自动的 koajs 应用程序，只需简单的配置，则可以得到一个强大的『配置优先的』Web 应用程序。

## 配置文件总览

先看看配置文件 /config/develepment.js

```javascript
module.exports = {
  debug                : true,
  app_name             : "cloudoll",
  my_host              : "localhost",
  port                 : 3000,
  controller_dirs      : ['/api/open', '/api/admin', '/api/inner'],
  schema_path          : './schema',
  my_errors_path       : './errors.js',
  koa_middles_forbidden: {
    clouderr_handle: true,
    auto_router    : true,
    json_validator : true,
    authenticate   : true
  },
  cloudeer             : {
    server             : "http://127.0.0.1:8801",
    not_a_consumer     : false,
    not_a_service      : false,
    no_methods_register: false
  }
};


```

在 linux 或者 mac 下，可以不用配置 my_host。

其实，所有的东西都可以不用配置哦。


## 根据不同的配置文件启动应用

下面的例子将启动 /config/product.js 配置文件

```javascript
var KoaApplication = require('cloudoll').KoaApplication;
process.env.NODE_ENV = "product";
var app = KoaApplication();
```

在实际应用中，可以把 NODE_ENV 配置到系统环境变量中。比如：

```bash
#!/usr/bin/bash

export NODE_ENV='product'
```

这样就可以不用修改项目里面的代码而加载不同的配置文件。

## 我可以独立使用吗？

可以！如果没有配置 cloudeer 节点，那么应用程序将会变成一个普通的 自动映射的 koa 框架。

## KoaApplication 包含如下的一些特性

### app.context 扩展方法

可以在路由方法中使用 this. 直接调用。

#### echo

在路由中，可以使用 this.echo("xxxx") 输出结果。

他会自动封装成

```json
{ "errno": 0, "data": "xxxx"}
```

#### *getCloudeer(service, url, params)

可以使用此方法直接调用一个远程微服务的 GET 方法，这是一个 generator。

如果没有配置 cloudeer 节点，那么将不会有这个方法。


#### *postCloudeer(service, url, params)

和 *getCloudeer  一样，调用远程微服务的 POST 方法。



上面三个方法的代码示例:

```
module.exports = {
  world: function *() {
    var dist = yield this.getCloudeer("cloudarling", "/open/district/children");
    this.echo(dist);
  }
};
```



### 自动映射 cloudeer 远程调用


程序自动映射了下面三个路由：


**POST /cloudeer**

参数示例：
```json
{
    "method": "GET",
    "service": "cloudarling",
    "url": "/open/account/login",
    "params": { "passport": "xxx", "password": "xxxxxxx" }
}
```


**POST /cloudeer/get**

参数和上面的原型一样。默认 method 为 GET。

**POST /cloudeer/post**

参数和上面的原型一样。默认 method 为 POST。

这个功能无法通过配置文件关闭，除非删除了 cloudeer 配置节点。


### 自动加载错误集合

KoaApplication 自动加载了 cloudoll 的 errors 模块。

这个模块封装了 web 开发的常用的错误类型，方便开发者在程序中调用。

你也可以加载自定义的错误集合，只需要将下面的代码放到合适的位置就会自动加载。

代码示例如下：

```javascript
// 默认放在根目录下： errors.js
// 这个文件的文件名可以配置，配置节点为：
// my_errors_path: './my-errors.js'
module.exports = {
  CUSTOM00      : {errno: -1, errText: "%s"},
  SYSTEM_ERROR01: {errno: -10, errText: "系统错误"}
}

```

上面的示例中：

errors.CUSTOM00 需要通过方法调用。

```javascript
throw errors.CUSTOM00('我爱xxx');
```

errors.SYSTEM_ERROR01  直接调用，他已经是一个 Clouderr 的实例了。

```javascript
throw errors.SYSTEM_ERROR01;
```

聪明的我已经看出了，是否需要参数以及参数的个数 是和 %s 的个数相关的。

[更多的错误处理请看这个文档](./Clouderr.md)


### 错误的统一处理中间键

加载了两个中间件：errorsHandleAhead, errorsHandleBehind

可以通过配置文件关闭：

```javascript
koa_middles_forbidden: {
    clouderr_handle: true
}
```

具体参考 [这个文档](./Clouderr.md)


### 已经自动加载的中间键：

* koa-bodyparser [参见这里](https://github.com/koajs/bodyparser)

* queryStringParser 这个插件将 query string 作为 json 放入 this.qs 中。


### 加载额外前置中间键

有时候 koa 的中间键需要提前构造，比如 『静态文件』 服务。

前置中间键将放入构造函数中，示例如下：

```javascript
var cloudoll = require('cloudoll');

var serve = require('koa-static');
var app   = new cloudoll.KoaApplication({
  middles: [serve('./publish')]
});
```



### 权限验证中间件 authenticate

可以通过配置文件关闭：

```javascript
koa_middles_forbidden: {
    authenticate: true
}
```

使用权限验证插件的前提本服务必须同时是一个消费端（consumer）。

下面的配置节点必须为 false （或删除此配置）。

```javascript
cloudeer: {
    not_a_consumer: false
}
```

具体参考 这个文档

### 自动路由

KoaApplication 会自动扫描目录，将方法和 url 自动对应起来，就像「传统的 MVC 那样」。

这样，你就可以不用写那些 router 了。

你可以不用担心性能，因为扫描操作只会在程序启动的时候运行一次。

[具体参考这里](./autoRouters.md)

### 自动 json-schema 验证

### 自动提交 cloudeer 的服务注册

### 自动提交 cloudeer 的方法列表

### 自动下载 cloudeer 的服务列表


