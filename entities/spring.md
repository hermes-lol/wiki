---
title: Spring
created: 2026-04-22
updated: 2026-04-22
type: entity
tags: [java, spring, framework, computer-science]
sources: [raw/sites/javaguide.cn/docs/system-design/framework/spring/]
confidence: high
---

# Spring

## 概述
Spring 是 Java 平台的全栈轻量级框架，提供 IoC 容器、AOP、事务管理、Web 开发等完整解决方案。

## 核心模块

### 1. 核心容器
| 模块 | 功能 |
|------|------|
| spring-core | IoC/DI 基础 |
| spring-beans | Bean 工厂 |
| spring-context | 上下文 |
| spring-expression | SpEL 表达式 |

### 2. 其他模块
| 模块 | 功能 |
|------|------|
| spring-aop | 切面编程 |
| spring-tx | 事务管理 |
| spring-web | Web 支持 |
| spring-webmvc | MVC 框架 |
| spring-jdbc | JDBC 封装 |
| spring-orm | ORM 集成 |

## IoC 容器

### BeanFactory vs ApplicationContext
| 特性 | BeanFactory | ApplicationContext |
|------|------------|-------------------|
| 初始化 | 延迟 | 启动时 |
| 国际化 | 否 | 是 |
| 事件机制 | 否 | 是 |
| 资源访问 | 否 | 是 |

### Bean 生命周期
```
1. 实例化 (Instantiation)
2. 属性赋值 (Populate)
3. ��始化 (Initialization)
   - Aware 接口
   - BeanPostProcessor
   - @PostConstruct
4. 使用
5. 销毁 (Destruction)
   - @PreDestroy
   - DisposableBean
```

### Bean 作用域
| 作用域 | 说明 |
|--------|------|
| singleton | 单例 (默认) |
| prototype | 多例 |
| request | HTTP 请求 |
| session | HTTP 会话 |
| application | ServletContext |

## AOP (面向切面编程)

### 核心概念
| 概念 | 说明 |
|------|------|
| Aspect | 切面 |
| JoinPoint | 连接点 |
| Pointcut | 切点表达式 |
| Advice | 通知 |
| Target | 目标对象 |
| Proxy | 代理对象 |

### 通知类型
| 通知 | 注解 |
|------|------|
| 前置 | @Before |
| 后置 | @AfterReturning |
| 异常 | @AfterThrowing |
| 最终 | @After |
| 环绕 | @Around |

### 代理方式
- **JDK 动态代理** - 基于接口
- **CGLIB** - 基于继承

## Spring Boot

### 核心特性
- 自动配置 (@EnableAutoConfiguration)
- 起步依赖 (starter)
- 内嵌服务器
- 配置文件 (application.yml)

### 启动流程
```
1. SpringApplication.run()
2. 创建 ApplicationContext
3. 扫描 @Configuration
4. 自动配置
5. 刷新上下文
6. 启动完成
```

## Spring MVC

### 请求流程
```
请求 → DispatcherServlet → HandlerMapping → Controller
    → Service → DAO → 数据库
    → ViewResolver → View → 响应
```

### 常用注解
| 注解 | 用途 |
|------|------|
| @Controller | 控制器 |
| @RestController | REST 控制器 |
| @RequestMapping | 路由映射 |
| @GetMapping/@PostMapping | HTTP 方法 |
| @RequestParam | 请求参数 |
| @PathVariable | 路径变量 |
| @RequestBody | 请求体 |

## Spring Security

### 核心组件
- FilterChain (过滤器链)
- AuthenticationManager
- UserDetailsService
- PasswordEncoder

## 相关概念
- [[spring-boot]] - Spring Boot
- [[spring-cloud]] - Spring Cloud
- [[mybatis]] - ORM 框架
- [[hibernate]] - ORM 框架

## 参考文献
- JavaGuide: system-design/framework/spring/