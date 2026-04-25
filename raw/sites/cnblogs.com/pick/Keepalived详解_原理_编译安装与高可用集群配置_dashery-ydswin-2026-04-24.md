# Keepalived详解：原理、编译安装与高可用集群配置 - dashery

> Author: ydswin
> Date: 2025-12-09
> Source: https://www.cnblogs.com/ydswin/p/19326078

---

在高可用架构中，避免单点故障至关重要。Keepalived正是为了解决这一问题而生的轻量级工具。本文将深入浅出地介绍Keepalived的工作原理，并提供从编译安装到实战配置的完整指南。
1. Keepalived简介与工作原理
Keepalived是一个基于VRRP协议（虚拟路由冗余协议） 实现的高可用解决方案。它的核心目标是通过自动故障转移来确保服务的连续性。
1.1 核心思想：VRRP协议
想象一个场景：两台路由器提供相同的功能，一台是主节点（Master），另一台是备节点（Backup）。它们共同拥有一个虚拟IP地址（VIP，Virtual IP），客户端只访问这个VIP。 正常工作：主节点持有VIP并对外提供服务。它会周期性地向备份节点发送VRRP通告包（组播地址为224.0.0.18），宣告自己“健在”。
故障发生：当备份节点在指定时间内收不到主节点的VRRP通告包时，它会认为主节点发生了故障。
自动切换：此时，备份节点会根据优先级选举（优先级最高的备份节点获胜）接管VIP，并将其绑定到自己的网络接口上，从而无缝地接替主节点继续提供服务。这个过程对客户端来说是透明的，从而实现了高可用。 1.2 Keepalived的三大模块
Keepalived主要由三个模块构成： Core模块：作为核心，负责主进程的启动、维护及全局配置文件的加载和解析。
Check模块：负责健康检查，支持对负载均衡器后端的真实服务器进行TCP检查、HTTP_GET检查等，确保只有健康的服务器才接收流量。
VRRP模块：这是实现VRRP协议的功能模块，负责处理主备节点间的通信和状态切换。 2. 安装Keepalived
安装Keepalived主要有两种方式：通过系统包管理器安装和通过源代码编译安装。前者简单快捷，后者则能提供更新的版本和更灵活的定制选项。
2.1 YUM安装（快速上手）
在基于RHEL/CentOS的系统上，可以使用yum命令直接安装：
yum install keepalived -y 安装后，可以使用systemctl命令来管理服务：
systemctl start keepalived.service # 启动
systemctl enable keepalived.service # 设置开机自启
systemctl status keepalived.service # 查看状态 2.2 源码编译安装（推荐用于生产）
源码安装可以获得最新版本，并允许进行自定义配置。
步骤1：安装依赖包
编译前需要安装必要的开发工具和库：
yum install -y gcc openssl-devel libnl3-devel libnfnetlink-devel net-snmp-devel curl make 步骤2：下载、编译与安装
从官方下载源码包，然后进行编译安装。使用 --prefix 参数可以指定安装目录，便于管理。
# 进入常用源码目录，下载（请替换为最新稳定版链接）
cd /usr/local/src/
curl -O http://keepalived.org/software/keepalived-2.2.4.tar.gz
# 或使用 wget https://www.keepalived.org/software/keepalived-2.2.4.tar.gz # 解压并进入目录
tar xvf keepalived-2.2.4.tar.gz
cd keepalived-2.2.4 # 配置、编译并安装
./configure --prefix=/usr/local/keepalived
make && make install 步骤3：配置系统服务
为了方便管理，需要将Keepalived配置为系统服务。 将启动脚本复制到系统目录：
cp /usr/local/src/keepalived-2.2.4/keepalived/keepalived.service /usr/lib/systemd/system/ 重新加载systemd配置：
systemctl daemon-reload 3. 配置Keepalived实现主备高可用
下面以配置一个简单的主备高可用集群为例。假设我们有两台服务器： 主节点（Master）：物理IP为 192.168.10.11
备节点（Backup）：物理IP为 192.168.10.12
虚拟IP（VIP）：192.168.10.100 3.1 主节点（Master）配置
编辑配置文件 /etc/keepalived/keepalived.conf（如果源码安装，可能需要手动创建/etc/keepalived目录并将配置文件放置于此）：
! Configuration File for keepalived global_defs { router_id LVS_MASTER_01 # 本节点标识，建议唯一
} vrrp_instance VI_1 { state MASTER # 初始状态设为MASTER interface eth0 # 监听VRRP通告和绑定VIP的网卡名，请根据实际情况修改 virtual_router_id 51 # 虚拟路由ID，同一集群内主备节点必须相同（0-255） priority 100 # 优先级（1-254），主节点应高于备节点 advert_int 1 # 通告间隔（秒） unicast_src_ip 192.168.10.101 # 本机的真实IP地址 unicast_peer { 192.168.10.102 # 对端备节点的真实IP地址 } authentication { # 认证配置，主备需一致 auth_type PASS # 认证类型 auth_pass 1111 # 认证密码 } virtual_ipaddress { 192.168.10.100/24 # 定义的虚拟IP(VIP)，可多个 }
} 3.2 备节点（Backup）配置
备节点的配置与主节点相似，主要区别在于 state 和 priority。
! Configuration File for keepalived global_defs { router_id LVS_BACKUP_01 # 备节点标识
} vrrp_instance VI_1 { state BACKUP # 初始状态设为BACKUP interface eth0 virtual_router_id 51 # 必须与主节点相同 priority 90 # 优先级低于主节点 advert_int 1 unicast_src_ip 192.168.10.102 # 本机的真实IP地址 unicast_peer { 192.168.10.101 # 对端备节点的真实IP地址 } authentication { auth_type PASS auth_pass 1111 # 密码与主节点相同 } virtual_ipaddress { 192.168.10.100/24 }
} 3.3 启动服务并验证 启动服务：在主备节点上分别启动Keepalived。
systemctl start keepalived 检查虚拟IP：在主节点上执行 ip addr show eth0 命令，应该能看到VIP 192.168.10.100 已经绑定在 eth0 网卡上。
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000 link/ether 00:0c:29:xx:xx:xx brd ff:ff:ff:ff:ff:ff inet 192.168.10.11/24 brd 192.168.10.255 scope global noprefixroute eth0 valid_lft forever preferred_lft forever inet 192.168.10.100/24 scope global secondary eth0:0 valid_lft forever preferred_lft forever 模拟故障测试：将主节点的Keepalived服务停止（systemctl stop keepalived）或直接关闭主服务器。然后在备节点上再次执行 ip addr 命令，会发现VIP已经"漂移"到了备节点上。此时通过VIP访问服务，应仍然正常，从而实现了高可用。 4. 核心配置参数解析
下表总结了关键配置参数的含义： 参数项
含义与说明 global_defs
全局配置段 router_id
本机标识，通常使用主机名，用于在集群中区分不同节点 vrrp_instance
定义一个VRRP实例（一个虚拟路由器） state
初始状态，MASTER 或 BACKUP，但最终状态由优先级决定 interface
绑定VIP和发送VRRP通告的物理网卡 virtual_router_id
虚拟路由ID，同一组主备节点必须完全相同 priority
优先级（1-254），决定谁成为Master，值越大优先级越高 advert_int
主节点发送VRRP通告报文的时间间隔（秒） authentication
节点间通信认证，防止未经授权的节点加入 auth_type
认证方式，一般为 PASS（密码认证） auth_pass
认证密码，最多8位，主备节点必须一致 virtual_ipaddress
定义的虚拟IP地址，即VIP 5. 非抢占模式
默认情况下，Keepalived工作在抢占模式。这意味着当原Master节点恢复后，它会重新抢占VIP，夺回Master身份。在某些场景下，我们可能希望故障恢复后的节点作为新的备份，以避免服务因再次切换而波动。这时可以配置非抢占模式。
在vrrp_instance配置段中添加：
nopreempt # 启用非抢占模式 需要注意的是，在非抢占模式下，初始状态state建议都设置为BACKUP。
总结
Keepalived通过VRRP协议提供了一种简单而高效的高可用解决方案。从理解其核心原理到动手编译安装，再到根据实际需求配置主备或非抢占模式，您已经可以构建基础的高可用集群。在生产环境中，通常还会结合Nginx、LVS、HAProxy等负载均衡器，并编写自定义的健康检查脚本，以构建更加健壮和复杂的应用高可用架构。 本文来自博客园，作者：dashery，转载请注明原文链接：https://www.cnblogs.com/ydswin/p/19326078