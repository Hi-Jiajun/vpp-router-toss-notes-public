# vpp_router_toss_notes

VPP 家用主路由折腾笔记与复现文档。

当前这套方案的核心是：

- `VPP` 负责主数据面
  - `PPPoE`
  - `NAT44 EI`
  - `IPv6 PD`
  - Telegram IPv4/IPv6 静态路由
- `Linux` 负责控制面
  - `dnsmasq`
  - `LCP` 对出的 `lan0`
  - `vpphost0` 作为 Linux 通过 VPP 上网的内部出口

注意：

- 文档中已对敏感凭据做占位符处理
- 本仓库记录的是一套“当前能跑的实战配置”，不是通用最佳实践

## 目录

1. [拓扑与地址规划](./02.%E6%8B%93%E6%89%91%E4%B8%8E%E5%9C%B0%E5%9D%80%E8%A7%84%E5%88%92.md)
2. [VPP 主配置](./03.VPP_%E4%B8%BB%E9%85%8D%E7%BD%AE.md)
3. [dnsmasq 配置](./04.dnsmasq_%E9%85%8D%E7%BD%AE.md)
4. [Linux 通过 VPP 出网](./05.Linux_%E9%80%9A%E8%BF%87_VPP_%E5%87%BA%E7%BD%91.md)
5. [Telegram 静态路由](./06.Telegram_%E9%9D%99%E6%80%81%E8%B7%AF%E7%94%B1.md)
6. [sysctl 与 netplan](./07.sysctl_%E4%B8%8E_netplan.md)
7. [验证与排障](./08.%E9%AA%8C%E8%AF%81%E4%B8%8E%E6%8E%92%E9%9A%9C.md)

## 预留目录

- [src/README.md](./src/README.md)
- [img/README.md](./img/README.md)

