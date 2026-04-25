---
title: ArrayList
created: 2026-04-22
updated: 2026-04-22
type: entity
tags: [java, data-structure, collection, computer-science]
sources: [raw/sites/javaguide.cn/docs/java/collection/arraylist-source-code.md]
confidence: high
---

# ArrayList

## 概述
ArrayList 是 Java 中最常用的动态数组实现，继承自 AbstractList，实现了 List、RandomAccess、Cloneable、Serializable 接口。

## 核心特性

### 1. 底层结构
- 使用 Object[] elementData 数组存储元素
- 默认容量为 10
- 扩容机制：首次添加元素时容量扩展为 10，之后按 1.5 倍扩容

### 2. 线程安全性
- **非线程安全**
- 多线程环境下需要使用 Collections.synchronizedList() 或 CopyOnWriteArrayList

### 3. 核心操作复杂度
| 操作 | 时间复杂度 |
|------|-----------|
| add(E e) | O(1) 均摊 |
| add(index, E e) | O(n) |
| get(index) | O(1) |
| remove(index) | O(n) |
| contains(Object o) | O(n) |

## 源码关键点

### 扩容源码逻辑
```java
private void grow(int minCapacity) {
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);  // 1.5倍
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```

### ModCount 快速失败机制
- 迭代过程中如果数组被修改，抛出 ConcurrentModificationException
- 这是 fail-fast 机制，不保证线程安全，只作为错误检测

## 与 LinkedList 对比

| 特性 | ArrayList | LinkedList |
|------|-----------|------------|
| 访问方式 | 随机访问 O(1) | 遍历 O(n) |
| 头部插入/删除 | O(n) | O(1) |
| 尾部插入/删除 | O(1) 均摊 | O(1) |
| 内存占用 | 连续内存 | 额外指针开销 |
| 缓存友好性 | 高 | 低 |

## 适用场景
- 随机访问元素频繁
- 主要是尾部添加/删除
- 不需要线程安全

## 相关概念
- [[hashmap]] - HashMap 是另一个常用集合
- [[linked-list]] - LinkedList 是链表实现
- [[java-collections]] - Java 集合框架概述
- [[arraylist-vs-linkedlist]] - 详细对比

## 参考文献
- JavaGuide: arraylist-source-code.md