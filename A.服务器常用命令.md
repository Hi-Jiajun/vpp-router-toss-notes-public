# 服务器常用命令

本章记录当前这套路由虚拟机最常用的一批命令，主要用于：

- 日常检查
- 快速排障
- 服务验证
- 配置生效后的状态确认

参考结构来源：

- [A.服务器常用命令.md](https://gitee.com/callmer/linux_router_toss_notes/blob/main/A.%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4.md)

但内容已经改成更贴近当前这套 `VPP + Linux` 路由环境的常用命令集。

## 🧠 VPP

```bash
# 查看 VPP 版本
vppctl show version

# 查看接口概要
vppctl show interface

# 查看接口地址
vppctl show interface address

# 查看硬件接口
vppctl show hardware-interfaces

# 查看 LCP
vppctl show lcp

# 查看 PPPoE client 详情
vppctl show pppoe client detail

# 查看 NAT44 EI 接口
vppctl show nat44 ei interfaces

# 查看 NAT44 EI sessions
vppctl show nat44 ei sessions

# 查看 IPv4 FIB
vppctl show ip fib

# 查看 IPv6 FIB
vppctl show ip6 fib

# 查看错误计数
vppctl show errors

# VPP 发起 IPv4 连通性测试
vppctl ping 223.5.5.5

# VPP 发起 IPv6 连通性测试
vppctl ping ipv6 2400:3200::1 source lan0phy repeat 3
```

## 🌐 网络

```bash
# 查看本机网卡概览
ip -br link

# 查看本机地址概览
ip -br a

# 查看指定接口地址
ip -br a show lan0
ip -br a show vpphost0

# 查看 IPv4 路由表
ip route

# 查看 IPv6 路由表
ip -6 route

# 查看 IPv6 邻居
ip -6 neigh

# 查看 lan0 上的 router 邻居
ip -6 neigh show dev lan0 | grep router

# 查看 PCI 设备
lspci -nn

# 查看 networkd 状态
networkctl

# 查看某个接口详细状态
networkctl status enp1s0
networkctl status enp2s0
```

## 🔎 DNS 与解析

```bash
# 语法检查 dnsmasq
dnsmasq --test

# 直接用系统 resolver 查询
getent hosts www.baidu.com

# 使用 nslookup 测试上游
nslookup www.baidu.com 192.168.1.1
nslookup www.baidu.com 192.168.1.2
nslookup www.baidu.com 192.168.1.250

# 测试 IPv6 AAAA 解析
nslookup -type=AAAA www.qq.com 192.168.1.1
```

## 🧪 连通性测试

```bash
# 测试 Linux 到 VPP 内部通道
ping -c 3 10.255.255.1

# 测试 Linux 自身出网
ping -c 3 223.5.5.5
curl -4 -I --max-time 8 https://www.baidu.com

# 测试 LAN 侧地址
ping -c 3 192.168.1.1

# 测试辅助节点
ping -c 3 192.168.1.2
ping -c 3 192.168.1.4
ping -c 3 192.168.1.250
```

## 🪵 日志

```bash
# 查看系统日志尾部
journalctl -e

# 查看本次启动日志
journalctl -b

# 查看 VPP 日志尾部
tail -n 80 /var/log/vpp/vpp.log

# 持续跟踪 VPP 日志
tail -n 30 -f /var/log/vpp/vpp.log

# 查看 dnsmasq 日志尾部
tail -n 80 /var/log/dnsmasq.log

# 持续跟踪 dnsmasq 日志
tail -n 30 -f /var/log/dnsmasq.log
```

## 🖥️ 系统

```bash
# 查看时间
date

# 查看时区
date -R
timedatectl

# 查看磁盘占用
df -hT

# 查看 inode 占用
df -hiT

# 查看内存
free -h

# 查看 CPU / 内存 / 负载
top

# 如果安装了 btop
btop

# 查看系统参数
sysctl -a

# 重新加载 sysctl
sudo sysctl --system

# 生成 netplan
sudo netplan generate

# 应用 netplan
sudo netplan apply
```

## 🔧 服务

```bash
# 查看 networkd
systemctl status systemd-networkd.service

# 查看 qemu guest agent
systemctl status qemu-guest-agent

# 查看 dnsmasq
systemctl status dnsmasq

# 查看 VPP
systemctl status vpp

# 重启 dnsmasq
sudo systemctl restart dnsmasq

# 重启 VPP
sudo systemctl restart vpp

# 重新加载 systemd
sudo systemctl daemon-reload
```

## 🧭 路由与辅助节点

```bash
# 查看 Telegram IPv4 路由下一跳
vppctl show ip fib | grep '192.168.1.4'

# 查看 Telegram IPv6 路由下一跳
vppctl show ip6 fib | grep 'fd00:1234:5678:1::254'

# 查看 fakeip 路由
vppctl show ip fib | grep '7.0.0.0/8'
```

## 🚨 故障前取证

下面这组命令适合在“怀疑网络异常，但还没决定重启服务”时先保存现场：

```bash
date
vppctl show pppoe client detail
vppctl show interface address
vppctl show nat44 ei sessions
vppctl show interface
vppctl show errors | sed -n '1,160p'
ip -br a
ip route
ip -6 route
ip -6 neigh show dev lan0 | grep router || true
tail -n 80 /var/log/vpp/vpp.log
tail -n 80 /var/log/dnsmasq.log
```

