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
