---
title: Keepalived详解：原理、编译安装与高可用集群配置
source_url: https://www.cnblogs.com/ydswin/p/19326078
author: ydswin
date: 2026-04-25
category: 后端
tags: [运维, 高可用, VRRP, Keepalived]
---

# Keepalived详解：原理、编译安装与高可用集群配置

## VRRP 协议

两台路由器（Master + Backup）共享一个虚拟 IP（VIP），客户端只访问 VIP。VRRP 组播地址：`224.0.0.18`

## 三大模块

1. VRRP 协议栈 — 虚拟路由器选举和故障转移
2. 健康检查模块 — 监控后端服务
3. 配置管理模块 — 解析配置

## 源码编译安装（推荐生产）

```bash
yum install -y gcc openssl-devel libnl3-devel libnfnetlink-devel net-snmp-devel curl make
cd /usr/local/src/
curl -O http://keepalived.org/software/keepalived-2.2.4.tar.gz
tar xvf keepalived-2.2.4.tar.gz && cd keepalived-2.2.4
./configure --prefix=/usr/local/keepalived
make && make install
```

## 主备配置要点

| 参数 | 说明 |
|------|------|
| virtual_router_id | 主备必须相同 |
| priority | 主节点高于备节点 |
| unicast_src_ip | 本机真实 IP |
| nopreempt | 非抢占模式 |

## 非抢占模式

默认抢占模式：Master 恢复后重新抢 VIP。启用 `nopreempt` + state 都设 BACKUP 可避免服务波动。
