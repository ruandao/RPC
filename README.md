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
    
需要考虑的问题:

    1. 服务的提供者,可能是有状态的, 请求需要发给指定的 backend , 有状态的话,订阅信息,不适宜直接发给客户端(只能存放在 gate 里面)
    2. 服务的提供者,是无状态的的时候, 请求要发给负载均衡
    3. 注册中心,不能有单点失败问题, 需要高可用的保证   
    
    