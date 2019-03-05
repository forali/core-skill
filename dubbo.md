# dubbo学习 #

## 架构演变 ##
1. 单点系统。所有功能代码都在一个工程中，容易代码冲突。
2. 分布式系统。将一个项目分为多个子项目，如：客户系统、订单系统。
3. soa面向服务架构。将一个项目拆分成多个服务（去掉controller层），每个服务对外提供接口，rpc调用，返回http+xml形式数据（较重）。
4. 微服务架构。同上，返回http+json（较轻），服务划分更加细致。

综上的系统架构演变是逐层相关、逐层进化的。

## 分布式领域知识 ##
**角色：**

1. provider生产者（提供服务）
2. consumer消费者（调用服务）
3. registry注册中心（负载均衡、容错机制、路由、服务治理）
4. monitor监控中心（服务调用次数）
5. container容器（保存生产者信息）

**技术：**

1. HttpClient
2. Dubbo(Dubbox)
3. Zookeeper
4. Spring Cloud
5. RabitMQ(ActiveMQ、Kafka)
 
## 微服务架构 ##
**实现方式：**

1. Spring Cloud + http协议 + json格式
2. Dubbo

**架构图：**

![微服务架构图.png](https://i.imgur.com/HWp3IHs.png)

**<span style="color:red">重点：分布式事物</span>**

## Dubbo架构图 ##
![dubbo架构图](https://i.imgur.com/rFdNxU7.png)

provider到zookeeper中注册，就是在zookeeper上创建一个dubbo持久节点和服务节点（临时节点：因为），服务节点保存的是provider的ip和端口号。

## Dubbo功能 ##
1. rpc远程调用url管理（http://、tcp、rmi）
2. 负载均衡
3. 服务注册与发现