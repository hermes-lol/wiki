---
title: multi-task-watchdog.sh
source_url: (user provided)
ingested: 2026-04-23
---

#!/bin/sh

# multi-task-watchdog.sh
# -----------------------------------------------------------------------------
# 优化版：用于在 Docker 容器中启动和守护多个后台任务的纯 sh 脚本模板。
# 修复了旧版本的以下痛点：
# 1. 日志黑洞问题：舍弃 sed，改用 awk 并强制刷新缓冲区 (fflush)，实现日志零延迟。
# 2. 优雅退出 (Graceful Shutdown) 缺陷：使用 kill 0 机制向整个进程组广播信号，彻底杜绝孤儿进程和僵尸进程。
# 3. 暴力重启问题：引入智能指数退避策略，如果任务秒崩会逐渐拉长重启间隔，保护 CPU 且防止日志洪流。
# 4. 健壮性提升：加入 set -u 防止变量未定义导致静默失败。
# -----------------------------------------------------------------------------

# 使用未定义变量时直接报错退出
set -u

# ==========================================
# 1. 定义你的业务任务 (Dummy Tasks)
# ==========================================

task1_dummy() {
    echo "Initializing task 1..."
    sleep 2
    echo "Running task 1..."
    sleep 5
    echo "Oops! Task 1 crashed unexpectedly."
    return 1 # 模拟异常退出
}

task2_dummy() {
    echo "Starting task 2..."
    for i in 1 2 3 4 5; do
        echo "Task 2 is processing... (Heartbeat $i)"
        sleep 3
    done
    echo "Task 2 finished gracefully."
    return 0 # 模拟正常退出
}

# ==========================================
# 2. Watchdog 与日志处理核心逻辑
# ==========================================

run_and_watch() {
    task_name="$1"
    shift
    
    delay=1
    max_delay=30

    while true; do
        echo "[$task_name] (Re)starting command..."
        start_time=$(date +%s)
        
        "$@" 2>&1 | awk -v prefix="[$task_name]" '{print prefix, $0; fflush()}'
        
        end_time=$(date +%s)
        duration=$((end_time - start_time))
        
        if [ "$duration" -lt 5 ]; then
            delay=$((delay * 2))
            if [ "$delay" -gt "$max_delay" ]; then
                delay=$max_delay
            fi
            echo "[$task_name] WARNING: Exited too quickly (ran for ${duration}s). Backing off for ${delay}s..."
        else
            delay=1
            echo "[$task_name] Process exited cleanly or ran stable. Restarting in ${delay}s..."
        fi
        
        sleep "$delay"
    done
}

# ==========================================
# 3. 进程管理与优雅退出
# ==========================================

cleanup() {
    echo ""
    echo "[watchdog] Received termination signal (SIGTERM/SIGINT). Initiating graceful shutdown..."
    
    trap '' TERM INT
    kill -TERM 0 2>/dev/null
    wait
    
    echo "[watchdog] All tasks stopped cleanly. Exiting."
    exit 0
}

trap cleanup TERM INT

echo "[watchdog] Starting multi-task manager (Optimized Version)..."

# ==========================================
# 4. 启动任务
# ==========================================

run_and_watch "task1" task1_dummy &
run_and_watch "task2" task2_dummy &

echo "[watchdog] All tasks are running in background. Waiting for signals..."

while true; do
    wait
done