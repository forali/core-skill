## Nginx ##

 服务器，反向代理（不保真实ip地址）、虚拟主机，https，黑名单和白名单、csrf、xss、sql注入、ddos流量攻击、动静分离。


### 集群产生的问题？ ###
1. 分布式job幂等性问题（使用XXLjob分布式任务调度平台）
2. 会话共享
3. 分布式全局id（提前生成放入redis，分布式锁）

### 配置方向代理 ###
在nginx.conf文件中

	server {
		listen 80;
		// nginx地址
		server_name www.baidu.com;
		// 拦截所有请求
		location / {
			// 真正响应的ip
			proxy_pass http://localhost:8080
		}
	}

### 负载均衡 ###
1. 轮训
2. 权重
3. ip绑定，可解决session共享

**轮询配置：**

	upstream polling {
		server 127.0.0.1;
		server 127.0.0.2;
	}
	
	server {
		location / {
			proxy_pass http://polling;
		}
	}

**权重**

	upstream polling {
		server 127.0.0.1 weight=1;
		server 127.0.0.2 weight=2;
	}

**ip绑定**

	upstream polling {
		ip_hash;
		server 127.0.0.1;
		server 127.0.0.2;
	}

### 容错机制 ###
1. 一主一备
2. 多主多备

使用keepalived实现

### 跨域解决方案 ###
1. Nginx网关
2. Zuul网关
3. 接口中设置允许跨域请求
4. HttpClient内部转发
5. Jsonp

### 接口网关 ###
域名相同，根据项目名称不同来转发

	location /a {
		proxy_pass http://127.0.0.1:8001;
	}
	
	location /b {
		proxy_pass http://127.0.0.1:8002;
	}

### 集群session共享 ###
1. spring-session加上redis
2. nginx负载均衡策略之ip绑定
3. 数据库

### 高并发&高可用 ###
- 数据库
	- 优化sql，避免全表扫描
	- 使用索引
	- 分表分库
	- 读写分离
	- 主从复制
- 缓存，redis缓存
- 服务器
	- 方向代理
	- 配置负载均衡
	- 集群
	- CDN加速 
- 项目优化
	- 代码重构
	- jvm调优
	- 分布式