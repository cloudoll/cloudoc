## orm.redis
对redis访问做透明代理，目前只支持部分常用函数，设计初衷是redis 是key value形式的数据库，不同的业务设计防止出现数据覆盖。因此根据微服务划分命名空间，这样只需要保证每个微服务里的key不冲突就可以了，因此可以通过定义json的方式，json key在语法上要求名字唯一。正好满足需求，然后将这样key实例化成可访问redis的对象，这样代码里不用拼字符串key,直接使用json key就行。简单易用，易维护。
### 例子
订单号是存储在redis的一个数组，下单时需要从redis里取一个元素做为订单号
先假设redis里已经有了数据。


在项目根目录下新建一个redisKeys.js的文件，内容如下。


    module.exports = {
        order_no:{}
    };
使用方式
     let redis = require('cloudoll').orm.redis;
     let order_no = yield redis.order_no.lpop();
redis.order_no就是从redisKeys.js里实例化出来的redis访问对象,lpop函数是透明代理redis api的。假设微服务名是mall.service.order那么在redis实际访问的key是mall.service.order.order_no,值是一个字符数组[]
### string操作
set string
     let redis = require('cloudoll').orm.redis;
     yield redis.string_test.set('this is string');
     let string_value = yield redis.string_test.get();
string_value里的值就是this is string

### 对登录次数做限制，防止暴力破解
redisKeys.js
    module.exports = {
        login_count:{limit:5,expire:30*60}
    };
意思是半个小时内允许密码连续错误5次

使用方法
    let user_id=1;
    if(密码错误){
        yield redis.login_count.key(user_id).incr();
    }else{
        yield redis.login_count.key(user_id).del();
    }
