---
title: AQS
created: 2026-04-22
updated: 2026-04-22
type: entity
tags: [java, concurrency, computer-science]
sources: [raw/sites/javaguide.cn/docs/java/concurrent/aqs.md]
confidence: high
---

# AQS

## 概述
AQS (AbstractQueuedSynchronizer) 是 Java 并发包的核心抽象类提供基于 FIFO 队列的阻塞锁和同步器框架。

## 核心设计

### 1. 双向队列结构
```java
public abstract class AbstractQueuedSynchronizer {
    private volatile int state;  // 同步状态
    private transient Node head;  // 头结点
    private transient Node tail;  // 尾结点
}
```

### 2. Node 等待状态 (waitStatus)
| 值 | 含义 |
|---|------|
| 0 | 初始状态 |
| -1 | SIGNAL - 后继节点等待唤醒 |
| -2 | CONDITION - 条件等待 |
| -3 | PROPAGATE | 共享式传播 |
| 1 | CANCELLED | 已取消 |

### 3. 两种资源共享模式
- **Exclusive (独占式)** - 如 ReentrantLock
- **Shared (共享式)** - 如 CountDownLatch、Semaphore

## 核心方法

### 模板方法 (子类实现)
```java
protected boolean tryAcquire(int arg)      // 独占式获取
protected boolean tryRelease(int arg)     // 独占式释放
protected int tryAcquireShared(int arg)   // 共享式获取
protected boolean tryReleaseShared(int arg)// 共享式释放
protected boolean isHeldExclusively()     // 是否独占
```

### 锁获取模板
```java
public final void acquire(int arg) {
    if (!tryAcquire(arg) &&
        acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
        selfInterrupt();
}
```

## 底层实现

### 1. CAS 修改状态
```java
protected final boolean compareAndSetState(int expect, int update) {
    return unsafe.compareAndSwapInt(this, stateOffset, expect, update);
}
```

### 2. 队列获取锁
```java
final boolean acquireQueued(Node node, int arg) {
    for (;;) {
        Node p = node.predecessor();
        if (p == head && tryAcquire(arg)) {
            setHead(node);
            p.next = null;
            return false;
        }
        if (shouldParkAfterFailedAcquire(p, node))
            parkAndCheckInterrupt();
    }
}
```

## 常见实现类
- [[reentrant-lock]] - 可重入锁
- [[count-down-latch]] - 倒计时门闩
- [[semaphore]] - 信号量
- [[read-write-lock]] - 读写锁

## 相关概念
- [[reentrant-lock]] - 显式锁
- [[cas]] - CAS 无锁算法
- [[jmm]] - Java 内存模型

## 参考文献
- JavaGuide: aqs.md