---
---

# 项目摘要：Spring AI 智能面试平台 + RAG 知识库

> **来源**：掘金 | **作者**：JavaGuide | **阅读时间**：18分钟  
> **GitHub Stars**：450+（1个月内） | **Issues/PRs**：11个issue + 6个PR（完成率100%）

---

## 项目概述

基于 **Spring Boot 4.0 + Java 21 + Spring AI 2.0** 的 AI 智能面试辅助平台，完全免费开源，无Pro版或付费版。

### 三大核心功能

1. **智能简历分析**：上传简历 → AI多维度评分 + 改进建议
2. **模拟面试系统**：基于简历生成个性化面试题，支持实时问答和答案评估
3. **RAG 知识库问答**：上传技术文档构建私有知识库，支持向量检索增强的智能问答

### 项目地址

- GitHub：[github.com/Snailclimb/interview-guide](https://github.com/Snailclimb/interview-guide)
- Gitee：[gitee.com/SnailClimb/interview-guide](https://gitee.com/SnailClimb/interview-guide)

---

## 已完成优化（社区贡献）

- **API限流保护**：基于Redis+Lua封装分布式限流组件，支持按用户/IP/全局维度
- **前端性能优化**：RAG聊天界面引入虚拟列表；懒加载+代码分割解决首屏加载慢
- **功能优化**：向量功能+Tika简历解析优化；面试问题去重
- **Docker快速部署**：Docker Compose一键搭建全套运行环境

---

## 系统架构

### 分层结构

```
前端展示层 (React + TypeScript)
        ↓
后端服务层 (Spring Boot 4.0)
  ├── REST Controllers (统一API入口)
  ├── 业务服务层
  │   ├── Resume Service (简历上传/解析/AI分析)
  │   ├── Interview Service (面试会话/问题生成/答案评估)
  │   ├── Knowledge Service (知识库上传/文本分块/向量化)
  │   └── RAG Chat Service (检索增强生成/流式问答)
  ├── 异步处理层 (Redis Stream消费者)
  └── AI集成层 (Spring AI + DashScope/通义千问)
        ↓
数据存储层
  ├── PostgreSQL + pgvector (关系数据 + 向量检索)
  ├── Redis (会话缓存 + 消息队列)
  └── RustFS/MinIO (S3对象存储)
```

### 异步处理流程

```
简历分析/知识库向量化 → Redis Stream → 异步消费者 → 状态流转
状态：PENDING → PROCESSING → COMPLETED / FAILED
```

### 知识库问答流程

```
用户提问 → 向量检索 → 构建Prompt → LLM生成回答 → SSE流式返回
```

---

## 技术栈

### 后端

| 技术 | 版本 | 说明 |
|------|------|------|
| Spring Boot | 4.0 | 应用框架 |
| Java | 21 | 开发语言 |
| Spring AI | 2.0 | AI集成框架 |
| PostgreSQL + pgvector | 14+ | 关系数据库 + 向量存储 |
| Redis | 6+ | 缓存 + 消息队列(Stream) |
| Apache Tika | 2.9.2 | 文档解析 |
| iText 8 | 8.0.5 | PDF导出 |
| MapStruct | 1.6.3 | 对象映射 |
| Gradle | 8.14 | 构建工具 |

### 前端

| 技术 | 版本 | 说明 |
|------|------|------|
| React | 18.3 | UI框架 |
| TypeScript | 5.6 | 开发语言 |
| Vite | 5.4 | 构建工具 |
| Tailwind CSS | 4.1 | 样式框架 |
| React Router | 7.11 | 路由管理 |
| Framer Motion | 12.23 | 动画库 |
| Recharts | 3.6 | 图表库 |
| Lucide React | 0.468 | 图标库 |

---

## 技术选型深度解析

### 为什么选择 Spring AI？

> "Spring AI 是 Spring 官方推出的 AI 集成框架，提供了统一的 LLM 调用抽象。"

- **统一抽象**：一套代码支持多种LLM（OpenAI、阿里云DashScope、Ollama等），切换模型只需改配置
- **Spring生态集成**：与Spring Boot无缝集成
- **内置向量存储支持**：原生支持pgvector、Milvus、Pinecone
- **结构化输出**：通过`BeanOutputConverter`将LLM输出直接映射为Java对象

```java
// Spring AI 结构化输出示例
var converter = new BeanOutputConverter<>(ResumeAnalysisResult.class);
String result = chatClient.prompt()
    .user(userMessage)
    .outputConverter(converter)
    .call()
    .content();
// 直接得到 Java 对象
```

### 为什么选择 PostgreSQL + pgvector？

**方案对比**：

| 方案 | 优点 | 缺点 |
|------|------|------|
| PostgreSQL + pgvector | 一套数据库搞定，运维简单 | 向量检索性能不如专业库 |
| PostgreSQL + Milvus | 向量检索性能更好 | 多组件，运维复杂 |
| PostgreSQL + Pinecone | 云托管，无需运维 | 成本高，数据在第三方 |

**选择理由**：
- 架构简单，降低运维复杂度
- HNSW索引支持毫秒级检索，万级文档够用
- 事务一致性：向量数据和业务数据在同一数据库
- 支持SQL条件过滤（如"只在Java分类中检索"）

```sql
-- pgvector 相似度搜索示例
SELECT 1 - (embedding <=> query_embedding) as similarity
FROM knowledge_docs
WHERE metadata->>'category' = 'Java'
ORDER BY embedding <=> query_embedding
LIMIT 5;
```

> **为什么不选MySQL+向量数据库？** PostgreSQL的可扩展性是其王牌——pgvector（向量检索）、pg_bm25（全文搜索）、TimescaleDB（时序数据）、PostGIS（地理信息）等插件使其成为"数据瑞士军刀"，简化技术栈。

### 为什么引入 Redis？

两个核心场景：
1. **会话缓存**：替代`ConcurrentHashMap`
2. **异步消息队列**：基于Redis Stream实现简历分析、知识库向量化等耗时任务（10-60秒）

**Redis Stream vs 其他消息队列**：

| 维度 | Redis Stream | RabbitMQ | Kafka | 内存队列 |
|------|-------------|----------|-------|---------|
| 吞吐量 | 高（十万级QPS） | 中（万级） | 极高（百万级） | 极高 |
| 延迟 | 极低（亚毫秒） | 低（毫秒） | 中（毫秒级） | 极低 |
| 持久化 | 支持 | 支持 | 强支持 | 无 |
| 消息堆积 | 一般（内存限制） | 中 | 极强（TB级） | 差 |
| 运维复杂度 | 低 | 中 | 高 | 极低 |

**选择理由**：复用现有Redis组件，功能满足需求（消费者组、ACK、持久化），运维简单。

### 为什么使用 MapStruct？

| 方案 | 性能 | 类型安全 | 使用复杂度 |
|------|------|---------|-----------|
| MapStruct | 零反射，最快 | 编译时检查 | 定义接口即可 |
| BeanUtils | 反射，慢 | 运行时报错 | 一行代码 |
| ModelMapper | 反射，较慢 | 运行时报错 | 配置复杂 |
| 手写转换 | 最快 | 编译时检查 | 重复代码多 |

### 为什么使用 SSE 而不是 WebSocket？

| 方案 | 优点 | 缺点 |
|------|------|------|
| SSE | 简单，基于HTTP，单向推送 | 仅支持服务端→客户端 |
| WebSocket | 双向通信，功能强大 | 协议复杂，需维护连接状态 |

**选择理由**：LLM流式输出是单向的（服务端→客户端），SSE天然支持重连、跨域，Spring支持好（`Flux<SseEvent>`一行搞定）。

### 构建工具为什么选择 Gradle？

> "SpringBoot官方现在用的就是Gradle，加上国内现在都是Maven更多，换个Gradle还更新颖一些。"

---

## 配套教程内容

**付费教程**（更新进度已过大半），包含：

### 环境搭建
1. 本地搭建PostgreSQL + pgvector
2. Spring Boot + RustFS构建S3兼容对象存储
3. 大模型API申请和Ollama部署本地模型
4. 环境搭建终章与项目启动

### 核心功能开发
1. 简历上传、多格式内容提取与解析
2. Spring AI与大模型集成
3. Spring AI + pgvector实现RAG知识库问答
4. 生产级结构化Prompt编写
5. AI模拟面试功能
6. 基于iText 8的PDF报告导出
7. 基于SSE的打字机效果输出
8. Docker Compose一键部署

### 进阶优化
1. 统一异常处理与业务错误码设计
2. MapStruct实体映射最佳实践
3. 基于Redis Stream的异步任务处理
4. Spring Boot 4.0升级指南
5. Docker Compose一键部署

### 面试准备
1. 简历编写与项目经历深度包装指南（五大方向版本）
2. 面试官问"项目哪里来的"如何回答
3. Spring AI面试问题挖掘
4. RAG面试问题挖掘（3.4万字，35道高频题）
5. Redis面试问题挖掘
6. 文件上传和PDF导出面试问题挖掘

---

## 简历写法（五大方向）

1. **后端方向**：
   - 架构与分布式能力侧重
   - AI应用与响应式编程侧重
   - 工程化与基础设施侧重
2. **测试/测开方向**：
   - 单元测试与TDD
   - 功能/异常场景覆盖

> 每一条描述紧扣项目真实逻辑，并补充"用户认证与鉴权"等未涉及

[... summary truncated for context management ...]