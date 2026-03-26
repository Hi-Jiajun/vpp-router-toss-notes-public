# VPP 路由器折腾手记

## ✨ 介绍

基于 `VPP + Linux` 的家用主路由构建与折腾手记。

- 主数据面：
  - `VPP`
  - `PPPoE`
  - `NAT44 EI`
  - `IPv6 PD`
  - Telegram IPv4/IPv6 静态路由
- 控制面：
  - `Linux`
  - `dnsmasq`
  - `LCP` 对出的 `lan0`
  - `vpphost0` 作为 Linux 通过 VPP 上网的内部出口
- 部署形态：
  - `PVE` 中的专用路由虚拟机
  - `Ubuntu Server (minimized)`
  - 关键网口通过 `PCIe Passthrough` 交给虚拟机
- 适用环境：
  - 一台可运行 `PVE` 的宿主机，或同等虚拟化/物理环境
  - 至少两个网络口，分别承担 `WAN` 与 `LAN`
  - 上游连接方式为 `PPPoE`
  - 下游为独立的家庭 LAN
- 文档特点：
  - 以“当前能跑”的实战配置为主
  - 安装系统与安装软件部分参考了现成系列的章节结构，但仅保留适合当前环境的内容
  - 原始文档在私有仓库中维护，公开分享版本会额外做脱敏导出

### 🚀 推荐阅读顺序

1. 先看 `00-04`：把虚拟机、系统、软件、参数和 Linux 网络职责边界先搭好。
2. 再看 `05-09`：进入地址规划、`VPP` 主配置、`dnsmasq`、Linux 借道出网和 Telegram 静态路由。
3. 最后看 `10-12 + A`：用于验证排障、理解辅助节点、记录后续 backlog 和快速查命令。

### 📚 主线章节

1. [00. PVE 创建路由虚拟机](./00.PVE_创建路由虚拟机.md)
2. [01. Ubuntu 安装操作系统](./01.Ubuntu_安装操作系统.md)
3. [02. Ubuntu 安装系统软件](./02.Ubuntu_安装系统软件.md)
4. [03. Ubuntu 设置系统参数](./03.Ubuntu_设置系统参数.md)
5. [04. Ubuntu 设置系统网络](./04.Ubuntu_设置系统网络.md)
6. [05. 拓扑与地址规划](./05.拓扑与地址规划.md)
7. [06. VPP 主配置](./06.VPP_主配置.md)
8. [07. dnsmasq 配置](./07.dnsmasq_配置.md)
9. [08. Linux 通过 VPP 出网](./08.Linux_通过_VPP_出网.md)
10. [09. Telegram 静态路由](./09.Telegram_静态路由.md)
11. [10. 验证与排障](./10.验证与排障.md)

### 📎 补充章节

12. [11. 内网辅助节点说明](./11.内网辅助节点说明.md)
13. [12. 待完成功能与改进项](./12.待完成功能与改进项.md)

### 🧰 附录

14. [A. 服务器常用命令](./A.服务器常用命令.md)

### 📦 预留目录

- [`src/`](./src/README.md)：预留给可复用配置样例和脚本
- [`img/`](./img/README.md)：预留给拓扑图、截图和补充说明图片

### 📝 文章说明

1. 本系列文档涉及的参数需要按实际网络环境调整后使用。
2. 不同版本的 `VPP`、`Linux`、`dnsmasq`、`systemd-networkd` 和 `netplan` 表现可能略有差异。
3. 安装系统与安装软件两章仅保留当前环境下真正改动或需要说明的部分；未改动的标准安装步骤会直接引用参考链接。
4. 文档中已对敏感凭据做占位符处理；公开仓库中的地址、域名、设备名、MAC 等信息会额外做脱敏。
5. 本仓库记录的是一套实战可运行方案，不是通用最佳实践。

### ❤️ 赞助支持

如果这套笔记对你有帮助，欢迎请我喝杯咖啡。

<table>
  <tr>
    <td align="center">
      <img src="https://Hi-Jiajun.github.io/picx-images-hosting/wechat_qrcode.icohq9bcf.webp" alt="微信赞赏码" width="220" />
    </td>
    <td align="center">
      <img src="https://Hi-Jiajun.github.io/picx-images-hosting/alipay_qrcode.7p45v27tjq.webp" alt="支付宝收款码" width="220" />
    </td>
  </tr>
  <tr>
    <td align="center">微信</td>
    <td align="center">支付宝</td>
  </tr>
</table>

### ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=Hi-Jiajun/vpp-router-toss-notes-public&type=Date)](https://star-history.com/#Hi-Jiajun/vpp-router-toss-notes-public&Date)

