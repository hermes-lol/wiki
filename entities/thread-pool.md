---
title: ThreadPool
created: 2026-04-22
updated: 2026-04-22
type: entity
tags: [java, concurrency, thread, computer-science]
sources: [raw/sites/javaguide.cn/docs/java/concurrent/java-thread-pool-summary.md]
confidence: high
---

# ThreadPool

## 概述
线程池是一种线程复用机制，避免频繁创建销毁线程带来的开销提高响应速度和资源利用率。

## 核心组件

### 1. 线程池核心类
- **ThreadPoolExecutor** - 真正执行任务的线程池
- **Executors** - 工具类快速创建各类线程池

### 2. 线程池状态
| 状态 | 高3位 | 值 |
|------|------|-----|
| RUNNING | 111 | 正常接收任务 |
| SHUTDOWN | 000 | 不接收新任务但处理完 |
| STOP | 001 | 不接收不处理中断 |
| TIDYING | 010 | 任务完毕worker待清理 |
| TERMINATED | 011 | 完全终止 |

### 3. 构造参数
```java
public ThreadPoolExecutor(int corePoolSize,
                          int maximumPoolSize,
                          long keepAliveTime,
                          TimeUnit unit,
                          BlockingQueue<Runnable> workQueue,
                          ThreadFactory threadFactory,
                          RejectedExecutionHandler handler)
```

## 常见线程池

### Executors 创建的四种类型
| 类型 | 核心/最大 | 队列 | 适用场景 |
|------|----------|------|----------|
| newFixedThreadPool | n/n | LinkedBlockingQueue | 任务量大 |
| newCachedThreadPool | 0/Integer.MAX | SynchronousQueue | 短任务 |
| newSingleThreadExecutor | 1/1 | LinkedBlockingQueue | 串行执行 |
| newScheduledThreadPool | core/Integer.MAX | DelayedWorkQueue | 定时任务 |

## 任务执行流程

```
1. 线程数 < corePoolSize → 创建新线程执行
2. 线程数 >= corePoolSize → 加入队列
3. 队列满 且 线程数 < maxPoolSize → 创建新线程执行
4. 队列满 且 线程数 >= maxPoolSize → 拒绝策略
```

## 拒绝策略
| 策略 | 行为 |
|------|------|
| AbortPolicy | 抛 RejectedExecutionException |
| CallerRunsPolicy | 调用者线程执行 |
| DiscardPolicy | 丢弃任务不抛异常 |
| DiscardOldestPolicy | 丢弃最旧任务重试 |

## 核心方法
- **execute(Runnable)** - 提交任务无返回值
- **submit(Callable/Runnable)** - 提交返回 Future
- **shutdown()** - 优雅关闭
- **shutdownNow()** - 立即关闭

## 最佳实践
1. 不使用 Executors 静态方法创建线程池（队列无界可能导致 OOM）
2. 使用 ThreadPoolExecutor 手动设置参数
3. 合理设置 corePoolSize 和 maxPoolSize
4. 区分 CPU 密集型和 IO 密集型任务

## 相关概念
- [[concurrent-hash-map]] - 并发集合
- [[aqs]] - AQS 队列同步器
- [[jmm]] - Java 内存模型
- [[java-concurrent]] - Java 并发编程

## 参考文献
- JavaGuide: java-thread-pool-summary.md