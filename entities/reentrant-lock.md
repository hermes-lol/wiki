---
title: ReentrantLock
created: 2026-04-22
updated: 2026-04-22
type: entity
tags: [java, concurrency, lock, computer-science]
sources: [raw/sites/javaguide.cn/docs/java/concurrent/reentrantlock.md]
confidence: high
---

# ReentrantLock

## 概述
ReentrantLock 是 Java 中可重入的显式锁与 synchronized 关键字功能类似但更灵活。

## 与 synchronized 对比

| 特性 | synchronized | ReentrantLock |
|------|-------------|---------------|
| 锁获取 | 隐式 | 显式调用 |
| 释放方式 | 自动释放 | 必须在 finally 释放 |
| 公平锁 | 否 | 可配置 |
| tryLock | 否 | 支持 |
| 超时 lock | 否 | 支持 |
| 条件变量 | 内置一个 | 多个 Condition |
| 可中断 | 否 | 支持 |

## 核心API

```java
// 获取锁
void lock()
void lockInterruptibly()  // 可中断
boolean tryLock()
boolean tryLock(long time, TimeUnit unit)

// 释放锁
void unlock()

// 条件变量
Condition newCondition()
```

## ���平锁 vs 非公平锁

### 非公平锁 (默认)
- 尝试获取锁时直接插队
- 吞吐量高但可能饥饿

### 公平锁
- 按等待顺序获取锁
- 需传入 fair = true
- 吞吐量较低

```java
ReentrantLock lock = new ReentrantLock(true);  // 公平锁
```

## 可重入性
```java
lock.lock();
try {
    // 可以多次获取同一把锁
    lock.lock();  // 重入
    try {
        // 业务逻辑
    } finally {
        lock.unlock();  // 需要对应次数释放
    }
} finally {
    lock.unlock();
}
```

## 原理基于 [[aqs]]
- 内部维护 Sync 继承 AQS
- 锁获取：state + 1
- 锁释放：state - 1
- 可重入计数保存在 exclusiveOwnerThread

## 最佳实践
```java
ReentrantLock lock = new ReentrantLock();

lock.lock();
try {
    // 业务逻辑
} finally {
    lock.unlock();
}
```

## 相关概念
- [[aqs]] - AQS 同步器
- [[synchronized]] - 内置锁
- [[read-write-lock]] - 读写锁

## 参考文献
- JavaGuide: reentrantlock.md