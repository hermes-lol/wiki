---
tags: [多线程, 限制进程数, 有名管道, 文件描述符, 令牌, bash, shell, 并发控制]
source: 知乎
source_url: https://zhuanlan.zhihu.com/p/68574239
created: 2026-04-22
---

# Shell 多线程限制进程数

## 需求概述
[[Linux实现简单的多线程并行]]不能控制并行的任务数，直接使用容易被限速。本文采用有名管道（[[mkfifo]]）和文件描述符（[[fd]]）实现可限制进程数的多线程。

## 核心原理

使用有名管道（FIFO）作为令牌桶：
1. 创建 FIFO 文件，用文件描述符关联
2. 向 FIFO 中写入指定数量的字符（令牌）
3. 每个进程开始前先"取令牌"（read -u6）
4. 进程结束后"归还令牌"（echo >&6）
5. 当 FIFO 中没有令牌时，后续进程会等待

## 实现代码

```bash
#!/bin/bash
# bam to bed

start_time=`date +%s`  # 定义脚本运行的开始时间

tmp_fifofile="/tmp/$$.fifo"
mkfifo $tmp_fifofile   # 新建一个FIFO类型的文件
exec 6<>$tmp_fifofile  # 将FD6指向FIFO类型
rm $tmp_fifofile       # 删也可以，FD6已经打开了

thread_num=5  # 定义最大线程数

# 根据线程总数量设置��牌个数
# 事实上就是在fd6中放置了$thread_num个回车符
for ((i=0;i<${thread_num};i++));do
    echo
done >&6

for i in data/*.bam # 找到data文件夹下所有bam格式的文件
do
    # 一个read -u6命令执行一次，就从FD6中减去一个回车符，然后向下执行
    # 当FD6中没有回车符时，就停止，从而实现线程数量控制
    read -u6
    {
        echo "great" # 可以用实际命令代替
        echo >&6 # 当进程结束以后，再向FD6中加上一个回车符，即补上了read -u6减去的那个
    } &
done

wait # 要有wait，等待所有线程结束

stop_time=`date +%s` # 定义脚本运行的结束时间
echo "TIME:`expr $stop_time - $start_time`" # 输出脚本运行时间

exec 6>&- # 关闭FD6
echo "over" # 表示脚本运行结束
```

## 关键步骤解析

| 步骤 | 命令 | 作用 |
|------|------|------|
| 创建 FIFO | `mkfifo $tmp_fifofile` | 创建有名管道文件 |
| 关联 FD | `exec 6<>$tmp_fifofile` | 将 FD6 指向 FIFO |
| 初始化令牌 | `echo >&6` (循环) | 写入 N 个回车符作为令牌 |
| 获取令牌 | `read -u6` | 从 FD6 读走一个字符 |
| 归还令牌 | `echo >&6` | 向 FD6 写回一个字符 |
| 关闭 FD | `exec 6>&-` | 清理文件描述符 |

## 注意事项

- `rm $tmp_fifofile` 删除后 FD6 仍然可用，因为文件已被打开
- `wait` 命令确保所有后台进程完成
- 使用 `$$` 作为文件名后缀避免冲突

## 参考资料

[^1]: [Shell"多线程"，提高工作效率](https://zhuanlan.zhihu.com/p/68574239)