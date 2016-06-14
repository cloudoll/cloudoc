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
    server             : "http://112.74.29.211:8801",
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

