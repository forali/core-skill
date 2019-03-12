# Spring Cloud微服务架构 #

## Spring Cloud解决问题？ ##
- 配置管理
- 注册中心
- 服务注册与发现
- 断路器
- 路由策略
- 负载均衡
- 全局锁
- 接口网关
- 客户端调用
- 服务管理系统


rpc远程调用，是一套微服务解决方案。

## 主要步骤 ##
1. eureka注册中心搭建
	- 引入依赖（eureka）
	- 写配置文件（服务地址）
2. 服务提供者
	- 引入依赖（eureka、web）
	- 配置文件（eureka服务信息）
3. 服务消费者
	- 引入依赖（eureka、feign、ribbon负载均衡、web）
	- 配置文件（eureka）

## ribbon负载均衡 ##

1. 引入ribbon依赖
2. 在服务端调用类上面加上`@LoadBalanced`注解

## zuul接口网关 ##
拦截所有接口，解决跨域、参数拦截问题。类似nginx反向代理

1. 引入zuul、eureka依赖
2. 配置文件
3. @EnableZuulProxy开启注解

## config配置中心 ##
将配置文件存放在git、svn等分布式文件系统中，config项目缓存从git获取的配置信息供其他服务调用。

## feign客户端调用 ##
自带负载均衡

## Hystrix断路器 ##

1. 服务降级。失败则调用本地方法
2. 熔断机制、

解决服务雪崩效应（服务之间强依赖，一个服务错先错误会导致调用它的服务出现异常）。

## spring cloud与spring boot区别 ##
spring boot用于快速集成第三方框架，简化xml配置。

spring cloud微服务解决方案，依赖于spring boot。

## 如何使A服务只能被B服务调用 ##
使用zuul网关拦截所有请求，当访问A服务时判断是否请求者是不是B