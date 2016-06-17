# KoaApplication

自动的 koajs 应用程序，只需简单的配置，则可以得到一个强大的『配置优先的』Web 应用程序。

## 配置文件总览

先看看配置文件 /config/develepment.js

```
module.exports = {
  debug                : true,
  app_name             : "cloudoll",
  my_host              : "localhost",
  port                 : 3000,
  controller_dirs      : ['/api/open', '/api/admin', '/api/inner'],
  schema_path          : './schema',
  my_errors_path       : './my-errors.js',
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


## 根据不同的配置文件启动应用

下面的例子将启动 /config/product.js 配置文件

```
process.env.NODE_ENV = "product";
var koaApp = new KoaApplication();
```

在实际应用中，可以把 NODE_ENV 配置到系统环境变量中。比如：

```bash
#!/usr/bin/bash

export NODE_ENV='product'
```

这样就可以不用修改项目里面的代码而加载不同的配置文件。


## features

### 自动加载错误集合

提供了完整的快捷错误定义，方便程序中调用。

你也可以加载自定义的错误集合，代码示例如下：

```
// 默认放在根目录下： errors.js
// 这个文件的文件名可以配置，配置节点为：
// my_errors_path: './my-errors.js',

module.exports = {
  CUSTOM00      : {errno: -1, errText: "%s"},
  SYSTEM_ERROR01: {errno: -10, errText: "系统错误"}
}

```

上面的示例中：

errors.CUSTOM00 需要通过方法调用。

```
throw errors.CUSTOM00('我爱xxx');
```

errors.SYSTEM_ERROR01  直接调用，他已经是一个 Clouderr 的实例了。

```
throw errors.SYSTEM_ERROR01;
```

聪明的你可能已经看出，是否需要参数以及参数的个数 是和 %s 的个数相关的。

[更多的错误处理请看这个文档](./Clouderr.md)


### 错误的统一处理中间键

加载了两个中间件：errorsHandleAhead, errorsHandleBehind

可以通过配置文件关闭：

```
koa_middles_forbidden: {
    clouderr_handle: true
}
```

具体参考 [这个文档](./Clouderr.md)


### 已经自动加载的中间键：

* koa-bodyparser [参见这里](https://github.com/koajs/bodyparser）

* queryStringParser 这个插件将 query string 作为 json 放入 this.qs 中。




### 加载额外前置中间键

有时候 koa 的中间键需要提前构造，比如 『静态文件』 服务。

前置中间键将放入构造函数中，示例如下：

```
var cloudoll = require('cloudoll');

var serve = require('koa-static');
var app   = new cloudoll.KoaApplication({
  middles: [serve('./publish')]
});
```


### 自动映射 cloudeer 远程调用

这个功能无法通过配置文件关闭

程序自动映射了下面三个访问：


POST /cloudeer

参数示例：
```
{
    method: "GET",
    service: "cloudarling",
    url: "/open/account/login",
    params: { passport: 'xxx', password: 'xxxxxxx' }
}
```

POST /cloudeer/get

参数和上面的原型一样。默认 method 为 GET。

POST /cloudeer/post

参数和上面的原型一样。默认 method 为 POST。



### 权限验证中间件 authenticate

可以通过配置文件关闭：

```
koa_middles_forbidden: {
    authenticate: true
}
```

具体参考 这个文档


