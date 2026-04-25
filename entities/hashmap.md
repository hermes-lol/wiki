---
title: HashMap
created: 2026-04-22
updated: 2026-04-22
type: entity
tags: [java, data-structure, collection, computer-science]
sources: [raw/sites/javaguide.cn/docs/java/collection/hashmap-source-code.md]
confidence: high
---

# HashMap

## 概述
HashMap 是 Java 中最常用的键值对存储结构，基于哈希表实现，提供 O(1) 平均时间复杂度的增删改查操作。

## 核心特性

### 1. 底层结构 (JDK 1.8+)
- 数组 + 链表 + 红黑树
- 链表长度超过 8 时转为红黑树 (TREEIFY_THRESHOLD = 8)
- 红黑树节点数小于 6 时转回链表 (UNTREEIFY_THRESHOLD = 6)
- 数组初始化容量 16 (DEFAULT_INITIAL_CAPACITY)
- 负载因子 0.75 (DEFAULT_LOAD_FACTOR)
- 扩容阈值 = 容量 × 负载因子

### 2. 线程安全性
- **非线程安全**
- 多线程环境下使用 ConcurrentHashMap

### 3. 核心操作复杂度
| 操作 | 平均 | 最坏 |
|------|------|------|
| get(key) | O(1) | O(n) |
| put(key, value) | O(1) | O(n) |
| remove(key) | O(1) | O(n) |

## 源码关键点

### put 流程
```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}

final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    if (tab == null || (n = tab.length) == 0)
        n = (tab = resize()).capacity;
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);  // 新增节点
    else {
        // ... 链表/红黑树处理逻辑
    }
    return null;
}
```

### hash 计算优化
```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 :
        (h = key.hashCode()) ^ (h >>> 16);  // 高位参与运算
}
```

### 扩容机制
- 容量翻倍 (oldCap << 1)
- 节点重新定位：原位置或原位置 + 旧容量

## 与 Hashtable 对比

| 特性 | HashMap | Hashtable |
|------|---------|-----------|
| 线程安全 | 否 | 同步方法 |
| null key | 允许 | 不允许 |
| null value | 允许 | 不允许 |
| 性能 | 高 | 低 |
| 迭代安全 | fail-fast | 普通 |

## 适用场景
- 高效键值查找
- 不需要线程安全
- 作为缓存、索引等数据结构

## 相关概念
- [[arraylist]] - ArrayList 是数组实现
- [[concurrent-hash-map]] - 并发安全的 HashMap
- [[hashmap-put-process]] - put 流程详解
- [[java-collections]] - Java 集合框架

## 参考文献
- JavaGuide: hashmap-source-code.md