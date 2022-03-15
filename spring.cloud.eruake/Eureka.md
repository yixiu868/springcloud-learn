# Eureka

[SpringCloud基础](https://mp.weixin.qq.com/s/MJrahcDXwxgDr5zBdO3XWw)

## Eureka Server

```yaml
register-with-eureka: false # false表示不向注册中心注册自己
fetch-registry: false # false表示自己端就是注册中心，
```

## Eureka Client

某服务可能既是服务提供者又是服务消费者

如果单纯作为服务消费者，单纯的服务消费者无需对外提供服务，也就无需注册到Eureka中了。

```yaml
eureka:
  client:
    register-with-eureka: false # 当前微服务不注册到eureka
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/  
```

## Eureka治理机制

* 服务提供者

  * 服务注册

    启动的时候会`发送REST请求`的方式将自己注册到Eureka Server上，同时带上自身服务的一些`元数据信息`。

  * 服务续约

    在注册服务之后，服务提供者会维护一个心跳用来持续告诉Eureka Server：`我还活着`

  * 服务下线

    当服务实例进行正常的关闭操作时，会触发一个下线REST请求给Eureka Server，告诉服务注册中心：`我要下线了`

* 服务消费者

  * 获取服务

    在`启动服务消费者`的时候，它会发送一个`REST请求`给服务注册中心，来`获取上面注册的服务清单`。

  * 服务调用

    服务消费者在获取服务消费清单后，通过`服务名`可以获得具体提供服务的`实例名`和该实例的`元数据消息`，在进行服务调用的时候，`优先访问同处一个Zone`中的服务提供方。

* Eureka Server（服务注册中心）

  * 失效剔除

    默认每隔一段时间（默认60秒）将当前清单中超时（默认90秒）没有续约的服务剔除出去。

  * 自我保护

    Eureka Server在运行期间，会统计`心跳失败的比例`在`15分钟之内`是否`低于85%`。Eureka Server会将当前的实例注册信息保护起来，让这些实例不会过期，尽可能保护这些注册信息。