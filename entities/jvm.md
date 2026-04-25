---
title: JVM
created: 2026-04-22
updated: 2026-04-22
type: entity
tags: [java, jvm, virtual-machine, computer-science]
sources: [raw/sites/javaguide.cn/docs/java/jvm/]
confidence: high
---

# JVM

## 概述
JVM (Java Virtual Machine) 是 Java 程序运行的核心虚拟机，负责字节码执行、内存管理、垃圾回收等功能。

## 核心组件

### 1. 类加载子系统
```
 Bootstrap ClassLoader     (JAVA_HOME/jre/lib)
 Extension ClassLoader     (JAVA_HOME/jre/lib/ext)
 Application ClassLoader   (classpath)
```

### 2. 运行时数据区
| 区域 | 线程共享 | 用途 |
|------|---------|------|
| 程序计数器 | 否 | 字节码行号指示器 |
| 虚拟机栈 | 否 | 方法栈帧 |
| 本地方法栈 | 否 | Native 方法 |
| 堆 | 是 | 对象实例、数组 |
| 方法区 | 是 | 类信息、常量、静态变量 |

### 3. 执行引擎
- 解释器
- JIT 编译器
- GC 垃圾回收器

## 内存模型 (JMM)

### 主内存 vs 工作内存
```
主内存 (Main Memory) ←→ 工作内存 (Working Memory)
     ↑                         ↑
   read                    read/write
     ↑                         ↑
  变量值                  变量副本
```

### 8 种原子操作
| 操作 | 说明 |
|------|------|
| lock | 主内存变量标识为线程独占 |
| unlock | 释放线程独占的变量 |
| read | 主内存→工作内存 |
| load | 工作内存→变量副本 |
| use | 变量副本→执行引擎 |
| assign | 执行引擎→变量副本 |
| store | 工作内存→主内存 |
| write | 主内存→变量 |

### happens-before 规则
1. 程序顺序规则
2. 监视器锁规则
3. volatile 变量规则
4. 线程启动规则
5. 线程终止规则
6. 传递性

## 垃圾回收

### 分代收集
```
Young Gen (新生代)
  ├── Eden     (80%)
  └── Survivor (20%)
      ├── S0
      └── S1
Old Gen (老年代)
```

### 垃圾回收算法
1. **标记-清除** - 标记存活对象，清除未标记
2. **复制** - 存活对象复制到新区域
3. **标记-整理** - 标记后移动存活对象

### 常见 GC 组合
| Young | Old | 说明 |
|-------|-----|------|
| Serial | Serial Old | 单线程，最简单 |
| ParNew | CMS | 并行 + 并发 |
| Parallel Scavenge | Parallel Old | 吞吐量优先 |
| G1 | G1 | 整体标记整理 |
| ZGC | ZGC | 低延迟 |
| Shenandoah | Shenandoah | OpenJDK |

## 调优参数
```bash
# 堆大小
-Xms512m -Xmx512m

# 新生代
-Xmn256m

# 永久代 (JDK 7)
-XX:PermSize=256m -XX:MaxPermSize=256m

# 元空间 (JDK 8+)
-XX:MetaspaceSize=256m -XX:MaxMetaspaceSize=256m

# GC 选择
-XX:+UseG1GC

# 日志
-XX:+PrintGCDetails -Xloggc:gc.log
```

## 相关概念
- [[jmm]] - Java 内存模型
- [[class-loading]] - 类加载机制
- [[garbage-collection]] - 垃圾回收
- [[java-collections]] - Java 集合

## 参考文献
- JavaGuide: jvm/ 目录