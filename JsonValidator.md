## JsonValidator
在调用接口或者存储mongodb时使用[json schema](http://json-schema.org)自动验证数据的合法性
### 接口验证
接口验证只支持post json方式，假设有个下单接口/open/order/create按照约定，在schema下建立一个order-create.json的文件。内容就是接口的字段，可作为接口文档使用，具体如何写参考[json schema example](http://json-schema.org/examples.html)。然后就会自动验证数据类型，长度，是否存在等等。
### mongo数据验证
mongo没有像mysql这样的数据表约束，因此规定在存储时必须调用同一个方法。在这个方法里做json数据校验，防止非法数据进入数据库。同样是在schema文件夹下新建一个和mongo collection同名的文件比如order.json，那么使用时用BaseService.js作为访问入口，只有这里面的接口才使用order.json做验证。千万不要直接往mongo里面插入数据。

使用方式

```
    let BaseService       = doll.mongo.BaseService;
    let service           = new BaseService('order');
    let order={};
    yield service.save(order);
```

修改数据时同样也应该调用save方法做数据验证。
