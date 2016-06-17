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

**方法：fromJson(errJson)**

这是一个静态方法，用于从一个 json 中创建出 Clouderr

```javascript
var xerr = Clouderr.fromJson({errno:1, errText:'我爱xxx',service:'我的service'});
```


## 默认错误集合 errors

如果引用了 errors ，则会自动加载内置的错误集合。

```
var errors = require('cloudoll').errors;
```

这个错误的里有如下的错误定义：

```
module.exports = {
  CUSTOM               : {errno: -1, errText: "%s"},
  SYSTEM_ERROR         : {errno: -10, errText: "系统错误"},
  BAD_REQUEST          : {errno: -400, errText: "错误的 http 请求"},
  NOT_FOUND            : {errno: -404, errText: "您请求的 http 资源没有找到。"},
  INTERNAL_SERVER_ERROR: {errno: -500, errText: "内部服务器错误"},

  WHAT_REQUIRE           : {errno: -1001, errText: "缺少参数 %s"},
  WHAT_WRONG_RANGE       : {errno: -1002, errText: "%s 的取值范围错误。最小 %s，最大 %s"},
  WHAT_WRONG_FORMAT      : {errno: -1003, errText: "%s 格式不正确。"},
  WHAT_NOT_SAME          : {errno: -1004, errText: "输入的 %s 值不一致。"},
  WHAT_NOT_EXISTS        : {errno: -1005, errText: "%s 不存在。"},
  WHAT_TOO_MUCH          : {errno: -1006, errText: "%s 太多了。"},
  WHAT_TOO_LITTLE        : {errno: -1007, errText: "%s 太少了。"},
  WHAT_NOT_BELONGS_TO_YOU: {errno: -1008, errText: "%s 不属于你。"},
  WHAT_TOO_LONG          : {errno: -1009, errText: "%s 太长了，请不要超过 %s。"},
  WHAT_TOO_SHORT         : {errno: -1010, errText: "%s 太短了，请不要少于 %s。"},
  WHAT_EXISTED           : {errno: -1011, errText: "%s 已经存在。"},
  WHAT_OCCUPIED          : {errno: -1012, errText: "%s 被占用。"},
  WHAT_TIMEOUT           : {errno: -1013, errText: "%s 已超时。"},
  WHAT_EXPIRED           : {errno: -1014, errText: "%s 已过期。"},
  WHAT_ILLEGAL           : {errno: -1015, errText: "%s 不合法。"},
  WHAT_NOT_FOUND         : {errno: -1016, errText: "%s 没找到。"},
  WHAT_WRONG_LENGTH_RANGE: {errno: -1017, errText: "%s 的长度错误，最短是 %s，最长 %s。"},
  WHAT_WRONG_TYPE        : {errno: -1018, errText: "%s 的类型错误"},


  ACCESS_TOKEN_NOT_FOUND: {errno: -2001, errText: "access_token 不存在。"},
  ACCESS_TOKEN_EXPIRED  : {errno: -2002, errText: "access_token 已经过期。"},

  TICKET_EXPIRED      : {errno: -2050, errText: "票据已经过期，请重新获取。"},
  TICKET_VERIFY_FAILED: {errno: -2051, errText: "票据校验失败。篡改登录信息是违法行为！"},
  TICKET_ILLEGAL      : {errno: -2052, errText: "非法票据。篡改登录信息是违法行为！"},
  SIGN_VERIFY_FAILED  : {errno: -2053, errText: "签名验证失败。"},

  PASSWORD_NOT_STRONG  : {errno: -3001, errText: "密码太简单。%s"},
  CHINA_MOBILE_ILLEGAL : {errno: -3002, errText: "不正确的手机号码。"},
  EMAIL_ILLEGAL        : {errno: -3003, errText: "不正确的Email。"},
  CAPTCHA_VALIDATE_FAIL: {errno: -3004, errText: "验证码校验失败。"},
  PASSPORT_ILLEGAL     : {errno: -3005, errText: "不正确的登录凭据，必须是手机或者 Email。"},
  MEMBER_ONLY          : {errno: -3006, errText: "您还没有登录，请登录。"},
  NO_RIGHTS            : {errno: -3007, errText: "您没有权限。请联系管理员授权。"},

  LOGIN_ERROR                  : {errno: -3050, errText: '登录失败'},
  LOGIN_ERROR_BECAUSE          : {errno: -3051, errText: "登录失败。%s"},
  MEMBER_NOT_EXISTS            : {errno: -3052, errText: "用户不存在。"},
  MEMBER_NOT_APPROVED          : {errno: -3053, errText: "尚未审批通过，请耐心等待。"},
  MEMBER_FORBIDDEN             : {errno: -3054, errText: "您已经被系统禁止。"},
  MEMBER_LOGIN_TOO_MUCH_FAILURE: {errno: -3055, errText: "您登录失败的次数太多。暂时被系统锁定"},
  IP_LOGIN_TOO_MUCH            : {errno: -3056, errText: "您登录失败的次数太多。暴力破解他人密码是违法行为！"},

};

```


在应用程序中：

errors.CUSTOM 需要通过方法调用。

```
throw errors.CUSTOM('我爱xxx');
```

errors.SYSTEM_ERROR  直接调用，他已经是一个 Clouderr 的实例了。

```
throw errors.SYSTEM_ERROR;
```

聪明的你可能已经看出，是否需要参数以及参数的个数 是和 %s 的个数相关的。



**方法：success(data)**

这个有点奇怪，将输出一个『正确的』 json 结果。

```javascript
errors.success({a:1});
```

上面的代码将输入结果

```javascript
{errno: 0, data: {a:1}}
```

**方法：load(json, service)**

这个方法用于加载 json 配置文件。


## koa 中间件：errorsHandleAhead 和 errorsHandleBehind

```javascript

var KoaMiddle = require('cloudoll').KoaMiddle;
var koaMiddle = new KoaMiddle();

app.use(koaMiddle.errorsHandleAhead);

//....其他中间件

app.use(koaMiddle.errorsHandleBehind);

```

有了这两个中间键，我就可以在我的代码中随处 throw 了。

并且能得到正确的最终错误结果。
