---
title: Docker 多任务守护脚本分析
created: 2026-04-23
updated: 2026-04-23
type: query
tags: [software, shell, docker, devops, best-practices]
sources: [raw/snippets/multi-task-watchdog.md]
confidence: high
---

# Docker 多任务守护脚本分析

这是一个用于在 Docker 容器中启动和守护多个后台任务的纯 sh 脚本模板。

## 核心架构

```
┌─────────────────────────────────────────────────────┐
│                    main script                      │
│                  (set -u + trap)                    │
├─────────────────────────────────────────────────────┤
│  task1_dummy  │  task2_dummy  │  ... (可扩展)       │
│   &后台运行    │   &后台运行    │                     │
├─────────────────────────────────────────────────────┤
│              run_and_watch() x N                    │
│   (��个任务独立守护进程，智能重启+日志)              │
├─────────────────────────────────────────────────────┤
│              wait (主进程阻塞)                       │
│                   ↑                                 │
│            SIGTERM/SIGINT → cleanup()               │
│                   ↓                                 │
│            kill -TERM 0 (全进程组)                   │
└─────────────────────────────────────────────────────┘
```

## 四大优化点

| 痛点 | 旧方案 | 本脚本方案 | 原理 |
|------|--------|-----------|------|
| 日志延迟 | sed 缓冲 | awk + fflush() | 每行输出立即刷新缓冲区 |
| 孤儿进程 | 逐个 kill 子进程 | kill -TERM 0 | 向整个进程组广播信号 |
| 秒崩风暴 | 立即重启 | 指数退避 (delay *= 2) | 5秒内崩溃→拉长间隔 |
| 静默失败 | 无检查 | set -u | 未定义变量直接报错 |

## 关键代码解读

### 1. set -u 防止静默失败
```sh
set -u  # 使用未定义变量时脚本立即退出
```
避免 `$undefined_var` 静默展开为空字符串导致的逻辑错误。

### 2. kill -TERM 0 进程组广播
```sh
kill -TERM 0  # PID 0 = 当前进程组的所有进程
```
这是优雅退出的核心：主进程、后台任务、sleep、awk 管道全部会被同一信号终止，杜绝僵尸进程。

### 3. 指数退避策略
```sh
if [ "$duration" -lt 5 ]; then
    delay=$((delay * 2))  # 1→2→4→8→16→30 (上限)
fi
```
意图：任务 5 秒内崩溃 = 异常秒崩，可能配置错误或依赖缺失立即重试会 CPU 打满、日志洪泛。

### 4. awk + fflush() 日志零延迟
```sh
awk -v prefix="[$task_name]" '{print prefix, $0; fflush()}'
```
fflush() 强制每行立即刷盘解决管道缓冲延迟。

## 潜在问题

1. **wait 在某些 shell 可能被信号中断** — 当前用 trap 解决
2. **指数退避后重启成功 delay 不会自动恢复** — 实际上是对的，保持谨慎
3. **kill -TERM 0 在 PID namespace 中可能冲突** — 容器环境需注意

## 使用示例

```sh
# 添加新任务
run_and_watch "nginx" exec nginx -g "daemon off;" &
run_and_watch "cron"  exec crond -f &

# 生产环境建议
# - 添加健康检查接口
# - 日志轮转 (logrotate)
# - 进程监控指标暴露 (Prometheus)
```

## 总结

| 维度 | 评价 |
|------|------|
| 优雅退出 | ⭐⭐⭐⭐⭐ 业界最佳实践 |
| 日志实时性 | ⭐⭐⭐⭐⭐ awk + fflush |
| 防秒崩 | ⭐⭐⭐⭐ 指数退避合理 |
| 可扩展性 | ⭐⭐⭐ 需手动添加任务 |
| 兼容性 | ⭐⭐⭐⭐ 纯 sh，兼容性好 |

生产可用。核心设计（kill -TERM 0 + 指数退避）值得借鉴。

## 相关概念

- [[shell]] - Shell 脚本基础
- [[docker]] - Docker 容器化
- [[process-management]] - 进程管理
- [[design-patterns]] - 观察者模式在此脚本中的体现