## Zookeeper ##
>分布式开源框架，分布式协调工具

结构：树形结构

节点类型：

- 持久节点
- 临时节点
- 持久顺序节点
- 临时顺序节点

功能：

1. 注册中心
2. 负载均衡
3. 分布式锁


子节点不能重名。

### 分布式环境下生成唯一id？ ###
1. 提前生成好id存放到redis
2. 使用分布式锁（然后采用uuid或时间戳等方式）
	1. zookeeper，临时节点，谁创建这个临时节点成功就获取到锁，然后释放锁被其他节点监听到又开始抢锁。
	2. redis：set nx

**代码实现**

### zookeeper负载均衡 ###
**算法类型：**

1. 轮训
2. 权重
3. 绑定ip


### zookeeper选举机制 ###