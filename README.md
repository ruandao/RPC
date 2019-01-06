#分布式 RPC 库

底层基于 `MQ` 做为消息路由, 也可以支持自定义 `gate` (轻量的mq的实现)

序列化,支持 `protobuf` `json` 这两种, 需要的话,可以自行扩展

支持服务版本化, 同一个服务名称,不同版

支持多语言, `golang`, `js`

学习参考资料:

[游戏服务端究竟解决了什么问题？](https://www.cnblogs.com/fingerpass/p/game-server-programming-paradigm.html)

[RPC框架原理与实现](https://www.cnblogs.com/xiaoqi/p/java-rpc.html)


##实现路线:

    1. 先尝试自己实现
    2. 观看别人的实现
    3. 比对二者差异
    
预先考虑的问题:  

    1.会不会出现无效信息爆炸, 因为是基于 mq 的 pubsub 模式, 
    不是同一个 session 的信息, 需要在 broker 处就过滤掉,
    不发送到客户端才行, 即,协议上支持rpc
        
rpc 框架的角色:
    rpc 消费者, rpc提供者, 注册中心, 监控中心
    
问题:

    用户连接网关, 一个用户的数据,在网关可能转发到多个backend,
    且网关处,需要保存用户的session信息 
    有的数据只能是指定网关可以处理
    目前市面上的 rpc 框架是否支持这个模式?
    
    