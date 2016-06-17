# 错误处理模块 clouderr

有时候，我们需要将我们的错误信息统一处理。

在编码的过程中，我不希望例外情况打断我，我不要写那么多的 if else。

好吧，简单的把错误消息 throw 出去就好。

这个模块便是封装了大部分的例外情况，让我在编程的过程中简单的 throw 就好。

先看看我的代码：

```
var errors = require("cloudoll").errors;


// 这是 cloudoll 中的权限验证的中间件代码

  *authenticate(next) {
    var urls     = url.parse(this.url);
    var authCode = urls.pathname;
    authCode     = authCode.toLowerCase();
    if (Cloudeer.authUris.indexOf(authCode) < 0) {
      yield next;
    } else {
      if (process.env.debug) {
        logger.info("开始验证权限 %s", this.url);
      }
      var ticket = this.qs.ticket;
      if (!ticket) {
        throw errors.WHAT_REQUIRE('ticket');
      }

      var rights = yield this.app.cloudeer.invokeCo("GET", "cloudarling", "/open/account/rights",
        {
          ticket: ticket, service: process.env.app_name
        });

      var myRights = rights.rights;
      var godRight = myRights.filter(function (ele) {
        return ele.id == 0;
      });
      var isGods   = godRight && godRight.length > 0;
      if (!isGods) {
        var pathJson = url.parse(this.url);

        var myRight  = myRights.filter(function (ele) {
          return ele.code == authCode;
        });
        var hasRight = myRight && myRight.length > 0;
        if (!hasRight) {
          throw errors.NO_RIGHTS;
        }
      }
      yield next;
    }
  }

```

看看，所有的例外都是 throw 出去的。

## 错误类 Clouderr

这个类继承自内置的 Error, 有三个构造函数。

```
var err = new Clouderr(404, "网页找不到", "微服务名称");
```

微服务名称会默认使用 配置文件的 app_name 。如果没有则使用目录名。


## 默认错误集合 errors





