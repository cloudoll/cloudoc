# Cloudoll 套件

cloudoll 包含很多强大的功能，其中大多数功能都可以单独拿出来独立使用：

[初学者文档移到了 cloudoll 项目中](https://github.com/cloudoll/cloudoll)


## cloudoll 特性

* [KoaApplication](./KoaApplication.md)

* [错误/例外处理](./Clouderr.md)

* [自动路由](./autoRouters.md)

* [数据的简洁访问 mongo](orm.mongo.md) 

* [数据的简洁访问 mysql](orm.mysql.md) 

* [数据的简洁访问 postgres](orm.postgres.md) 

* 数据的简洁访问 redis

* 包含数据访问的服务层和控制层的基类

* schema 的自动验证

* [作为消费者调用远程其他微服务](./Cloudeer.md)

* 权限验证(依赖[帐号系统](https://github.com/cloudoll/cloudarling))

* 内网接口的权限验证

* cloudoll 项目中依赖的库可以直接被微服务引用

为了避免冲突，下面的类库直接可以从 cloudoll 中引用，而无需重新安装。类库的名称和 npm 是一致的。

暴露的命名空间是 cloudoll.libs：


```
cloudoll.libs = {
   co     : require('co'),
   koa    : require('koa'),
   mongodb: require('mongodb'),
   mysql  : require('mysql'),
   pg     : require('pg'),
   redis  : require('redis'),
   tracer : require('tracer'),
   request: require('request')
}
```


# 其他的相关或者依赖项目有：

* [注册中心 rest 版本](https://code.aliyun.com/cloudark/cloudeer)  

* [注册中心 tcp 长连接版本](https://code.aliyun.com/cloudark/cloudeer-server)

* [帐号系统](https://code.aliyun.com/cloudark/cloudarling)

* [JAVA 工具类](https://code.aliyun.com/cloudark/cloudoll-java)

* [Android 类库](https://code.aliyun.com/cloudark/cloudoll-android)

* 后台管理框架前端UI （正在创建中）



<a name='contributors' />

# 贡献者：

[啤酒云](https://code.aliyun.com/u/cloudbeer)

[Lin](https://code.aliyun.com/u/Lin)

[chuchur](https://code.aliyun.com/u/chuchur)

[黄兴](http://user.qzone.qq.com/1922327128)

[zhanxp](https://code.aliyun.com/u/zhanxp)