---
title: "Redis"
date: 2018-12-21T13:11:24+08:00
draft: true
---

### official

https://github.com/antirez/redis

https://redis.io/

> Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker.

https://redis.io/topics/introduction

http://try.redis.io/

https://redis.io/clients


#### install 

##### windows

https://github.com/MicrosoftArchive/redis/releases

download -> unzip -> mannually install it to windows service 手动注册为系统服务

```bash
cd "D:\Program Files\Redis"
"D:\Program Files\Redis\redis-server.exe" --service-run "D:\Program Files\Redis\redis.windows-service.conf"
```

#### node

http://redis.js.org/  
https://github.com/NodeRedis/node_redis

Node.js with Redis  
Server-side Javascript with Redis  
https://redislabs.com/lp/node-js-redis/

Using Redis with Node JS  
https://hackernoon.com/using-redis-with-node-js-8d87a48c5dd7

### 缓存

Redis新手指南：在node中使用redis  
https://segmentfault.com/a/1190000015882650




### 当作队列使用

Building a simple message queue using Redis server and Node.js  
https://medium.com/@weyoss/building-a-simple-message-queue-using-redis-server-and-node-js-964eda240a2a

用redis实现消息队列（实时消费+ack机制）  
https://segmentfault.com/a/1190000012244418

redis入门与应用教程  
http://redisbook.rails365.net/467181

redis 实现消息队列 (七)  
https://www.rails365.net/articles/redis-shi-xian-xiao-xi-dui-lie-qi