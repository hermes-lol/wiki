---
title: ConcurrentHashMap
created: 2026-04-22
updated: 2026-04-22
type: entity
tags: [java, concurrency, collection, computer-science]
sources: [raw/sites/javaguide.cn/docs/java/concurrent/concurrent-hash-map-source-code.md]
confidence: high
---

# ConcurrentHashMap

## 概述
ConcurrentHashMap 是 Java 并发包中线程安全的键值对容器 JDK1.5 引入 JDK1.8 重大改进入高性能。

## 与 HashMap 对比

| 特性 | HashMap | ConcurrentHashMap |
|------|---------|-------------------|
| 线程安全 | 否 | 是 |
| null key/value | 允许 | 不允许 |
| 迭代 | fail-fast | 安全迭代 |
| 复杂度 | O(1) 均摊 | O(1) 均摊 |

## JDK 1.8+ 底层结构
- 数组 + 链表 + 红黑树 (同 HashMap)
- 链表转红黑树阈值: 8
- 红黑树转链表阈值: 6

## 并发设计

### 1. 分段锁 vs synchronized
- JDK 1.7 及之前：Segment 分段锁数组每段一把锁
- JDK 1.8+：synchronized 锁头节点 + CAS 无锁操作

### 2. 核心并发操作

#### put 流程
```java
final V putVal(K key, V value, boolean onlyIfAbsent) {
    if (key == null || value == null) throw new NullPointerException();
    int hash = spread(key.hashCode());
    int binCount = 0;
    for (Node<K,V>[] tab = table;;) {
        Node<K,V> f; int n, i, fh;
        if (tab == null || (n = tab.length) == 0)
            tab = initTable();
        else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
            if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value, null)))
                break;
        }
        // ... 链表/红黑树处理
    }
    addCount(1, binCount);
    return null;
}
```

#### get 流程 (无锁)
```java
public V get(Object key) {
    Node<K,V>[] tab; Node<K,V> e, p; int n, eh;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (e = tabAt(tab, (n - 1) & (eh = spread(key.hashCode())))) != null) {
        if ((eh = e.hash) == key.hashCode() && 
            ((p = e.key) == key || (key != null && key.equals(p))))
            return e.val;
        // 红黑树或链表查找
    }
    return null;
}
```

### 3. CAS 操作
```java
static final <K,V> boolean casTabAt(Node<K,V>[] tab, int i,
                                     Node<K,V> c, Node<K,V> v) {
    return UNSAFE.compareAndSwapObject(tab, ((long)i << ASHIFT) + ABASE, c, v);
}
```

## 核心方法

| 方法 | 说明 |
|------|------|
| putIfAbsent(K, V) | 不存在则插入 |
| compute(K, BiFunction) | 计算新值 |
| merge(K, V, BiFunction) | 合并值 |
| forEach / map / reduce | 并行操作 |

## 与 SynchronizedMap 对比

```java
// 方式1: Collections 包装
Map m = Collections.synchronizedMap(new HashMap());

// 方式2: ConcurrentHashMap (推荐)
Map m = new ConcurrentHashMap<>();
```

| 指标 | synchronizedMap | ConcurrentHashMap |
|------|----------------|------------------|
| 并发度 | 1 把锁 | 多把锁 |
| 读性能 | 需同步 | 无锁 |
| 迭代安全 | 是 | 是 |

## 适用场景
- 高并发读写键值对
- 缓存、计数器、共享状态

## 相关概念
- [[hashmap]] - 普通 HashMap
- [[thread-pool]] - 线程池
- [[aqs]] - AQS 同步器

## 参考文献
- JavaGuide: concurrent-hash-map-source-code.md