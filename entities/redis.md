---
title: Redis
created: 2026-04-22
updated: 2026-04-22
type: entity
tags: [database, redis, computer-science]
sources: [raw/sites/javaguide.cn/docs/database/redis/]
confidence: high
---

# Redis

## 概述
Redis (Remote Dictionary Server) 是开源的内存数据结构存储，支持字符串、哈希、列表、集合、有序集合等多种数据类型，常用作缓存、消息队列、分布式锁等。

## 数据结构

### 1. 基础数据类型
| 类型 | 命令示例 | 用途 |
|------|----------|------|
| String | set/get | 缓存、计数器、限流 |
| Hash | hset/hget | 对象存储 |
| List | lpush/rpop | 消息队列、列表 |
| Set | sadd/smembers | 去重、标签 |
| ZSet | zadd/zrange | 排行榜、权重队列 |

### 2. 高级类型
- **Bitmap** - 位图，统计活跃用户
- **HyperLogLog** - 基数统计
- **Geospatial** - 地理位置
- **Stream** - 消息队列
- **Bloom Filter** - 布隆过滤器

## 持久化机制

### RDB (Redis Database)
```bash
# 触发条件
save 900 1      # 900秒内1个key变化
save 300 10     # 300秒内10个key变化
save 60 10000   # 60秒内10000个key变化
```
- 定时生成数据快照
- 文件紧凑，适合备份
- 可能丢失最近数据

### AOF (Append Only File)
```bash
# 同步策略
appendonly yes
appendfsync always     # 每次写
appendfsync everysec   # 每秒 (默认)
appendfsync no         # 操作系统
```
- 记录所有写操作
- 数据完整性强
- 文件较大

### 混合模式 (Redis 4.0+)
- RDB + AOF 增量

## 集群模式

### 1. 主从复制
```
Master ←←← Slave
  ↓         ↓
read     read
write    -
```

### 2. Sentinel (哨兵)
```
        Sentinel (主)
       ↙          ↘
   Sentinel     Sentinel
       ↓          ↓
    Master →←←← Slave
```

### 3. Cluster (集群)
- 16384 个槽位
- 节点间 Gossip 协议
- CRC16(key) % 16384 分配

## 缓存策略

### 淘汰策略
| 策略 | 说明 |
|------|------|
| volatile-lru | 有过期时间 + LRU |
| allkeys-lru | 所有 key + LRU |
| volatile-ttl | 有过期时间 + 最小 TTL |
| volatile-random | 随机 |
| noeviction | 不淘汰 |

### 缓存问题
1. **缓存穿透** - 布隆过滤器 + 空值缓存
2. **缓存击穿** - 互斥锁 + 永不过期
3. **缓存雪崩** - 随机过期时间 + 多级缓存

## 分布式锁

### SETNX 方案
```lua
-- 原子加锁
SET lock_key value NX EX 30

-- 释放锁 (Lua 脚本)
if redis.call("get", KEYS[1]) == ARGV[1] then
    return redis.call("del", KEYS[1])
else
    return 0
end
```

### RedLock (红锁)
- 5 个独立 Redis 实例
- 多数节点加锁成功

## 相关概念
- [[redis-data-structures]] - 数据结构详解
- [[redis-persistence]] - 持久化机制
- [[redis-cluster]] - 集群模式
- [[memcached]] - 对比 Memcached

## 参考文献
- JavaGuide: database/redis/