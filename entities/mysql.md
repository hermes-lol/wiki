---
title: MySQL
created: 2026-04-22
updated: 2026-04-22
type: entity
tags: [database, mysql, computer-science]
sources: [raw/sites/javaguide.cn/docs/database/mysql/]
confidence: high
---

# MySQL

## 概述
MySQL 是最流行的开源关系型数据库，Oracle 旗下产品，InnoDB 为默认存储引擎。

## 核心架构

### 1. 层级结构
```
连接层 → SQL 接口 → 解析器 → 优化器 → 执行器 → 存储引擎
```

### 2. 存储引擎
| 引擎 | 事务 | 锁粒度 | 特点 |
|------|------|--------|------|
| InnoDB | 是 | 行级 | 默认，支持 MVCC |
| MyISAM | 否 | 表级 | 全文索引 |
| Memory | 否 | 行级 | 内存存储 |
| Archive | 否 | 行级 | 压缩存储 |

### 3. InnoDB 特性
- MVCC (多版本并发控制)
- 事务 (ACID)
- 行级锁
- 外键约束
- 自适应哈希索引
- 脏页刷新 (doublewrite buffer)

## 索引结构

### B+ 树索引
```
        [根节点]
       /   |   \
   [中间节点] ... [中间节点]
    /  |  \        /  |  \
 [叶子节点]      [叶子节点]
    |              |
 [数据页] ←────→ [数据页]
```

### 索引类型
| 类型 | 说明 |
|------|------|
| 主键索引 | 唯一非空，B+ 树 |
| 唯一索引 | 唯一可 null |
| 普通索引 | 普通 B+ 树 |
| 全文索引 | FullText |
| 组合索引 | 最左前缀原则 |

## 事务隔离级别

| 级别 | 脏读 | 不可重复读 | 幻读 |
|------|------|-----------|------|
| READ UNCOMMITTED | √ | √ | √ |
| READ COMMITTED | × | √ | √ |
| REPEATABLE READ (默认) | × | × | √ |
| SERIALIZABLE | × | × | × |

### MVCC 原理
- 隐藏列：trx_id, roll_pointer
- Undo log 版本链
- Read View 快照读

## SQL 优化

### 1. 慢查询分析
```sql
-- 开启慢查询日志
slow_query_log=1
long_query_time=1
slow_query_log_file=/var/log/mysql/slow.log

-- 分析 EXPLAIN
EXPLAIN SELECT * FROM users WHERE id = 1;
```

### 2. 索引优化
- 遵循最左前缀
- 避免索引失效 (函数、类型转换、LIKE %xxx)
- 覆盖索引减少回表
- 避免 SELECT *

### 3. 分页优化
```sql
-- 低效
SELECT * FROM orders LIMIT 1000000, 10;

-- 高效 (基于主键)
SELECT * FROM orders WHERE id > 1000000 LIMIT 10;
```

## 锁机制

### 行锁 vs 表锁
- 行锁：InnoDB，支持并发
- 表锁：MyISAM，全表锁

### 死锁
```sql
-- 查看死锁
SHOW ENGINE INNODB STATUS;

-- 避免策略
1. 固定顺序获取锁
2. 降低隔离级别
3. 减小事务粒度
```

## 主从复制

### 原理
```
Master → Binlog → IO Thread → Relay Log → SQL Thread → Slave
```

### 复制方式
| 方式 | 优点 | 缺点 |
|------|------|------|
| 异步复制 | 性能高 | 可能丢失数据 |
| 半同步复制 | 数据不丢失 | 有延迟 |
| 全同步复制 | 数据一致 | 性能低 |

## 相关概念
- [[innodb]] - InnoDB 存储引擎
- [[mysql-index]] - MySQL 索引
- [[mysql-transaction]] - MySQL 事务
- [[mysql-lock]] - MySQL 锁机制

## 参考文献
- JavaGuide: database/mysql/