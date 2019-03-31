![image](https://note.youdao.com/yws/public/resource/67fb5567b76cfb8dbbabbcf29885ee4a/xmlnote/FF82B37579D444F490691A6C771254DF/11241)

## Eureka
- Eureka核心服务
```
服务注册中心：提供服务注册和服务发现
服务提供者：可以是spring boot 应用，也可以是其他遵循Eureka通信机制的应用。
服务消费者：消费者从服务注册中心获取服务列表，可以使用Ribbon或者Feign或者其他
```
- 当Ribbon与Eureka联合使用，Ribbon的服务实例清单RibbonServerList会被DiscoveryEnableNIWSServerList重写，扩展从Eureka注册中心获取服务端列表。
- Zone的设置可以在负载均衡是实现区域亲和特性:ribbon默认会优先访问同客户端处于一个Zone的服务端实例.根据这一特性可以设计有效的对区域性故障容错集群
- Eureka配置分为Client与Instance
```java
//作为注册中心
org.springframework.cloud.netflix.eureka.EurekaClientConfigBean
//作为服务提供/消费
org.springframework.cloud.netflix.eureka.EurekaInstanceConfigBean
```
```java
// 注册信息存储在双层Map
private final ConcurrentHashMap<String, Map<String, Lease<InstanceInfo>>> registry
            = new ConcurrentHashMap<>();
```
- Eureka 强调CAP的(AP-可用性与可靠性);Zookeeper 强调CAP的(CP-一致性和可靠性)
```
spring.cloud.loadbalancer.retry.enable=true     参数开启重试机制
```

## Ribbon
- Ribbon的核心服务
```
基于Http和Tcp的客户端负载均衡工具
```
- 使用Ribbon的客户端负载均衡的步骤
```
1. 服务提供者启动多个服务实例,注册到一个注册中心或高可用服务注册中心
2. 消费者直接通过调用被@LoadBalanced注解修饰过的RestTemplate来实现面向服务的接口掉用
```

## Hystrix
- Hystrix的核心服务
```
基于线程池的熔断,服务降级,请求缓存,请求合并

1. 防止单个依赖耗尽容器（例如 Tomcat）内所有用户线程
2. 降低系统负载，对无法及时处理的请求快速失败（fail fast）而不是排队
3. 提供失败回退，以在必要时让失效对用户透明化
4. 使用隔离机制（例如『舱壁』/『泳道』模式，熔断器模式等）降低依赖服务对整个系统的影响
5. 针对系统服务的度量、监控和报警，提供优化以满足近实时性的要求
6. 在 Hystrix 绝大部分需要动态调整配置并快速部署到所有应用方面，提供优化以满足快速恢复的要求
7. 能保护应用不受依赖服务的整个执行过程中失败的影响，而不仅仅是网络请求
```

## Feign
```
融合了Ribbon和Hystrix
```

## Zuul
```java
//请求过滤 
//接口的权限校验
//error过滤器
//动态路由 bootstrap.properties
public class AccessFilter extends ZuulFilter
```

## Config
```
远程的配置
```

## Bus
```
消息总线
```

## Stream
```
消息驱动
```

## Sleuth
```
分布式服务跟踪--联合ELK日志分析
```