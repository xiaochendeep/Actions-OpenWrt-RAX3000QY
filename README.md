# ImmortalWrt for CMCC RAX3000Q / RAX3000QY

基于 [kkstone/immortalwrt-ipq50xx](https://github.com/kkstone/immortalwrt-ipq50xx) 的 `openwrt-21.02` 分支，为 CMCC RAX3000Q / RAX3000QY 编译的 ImmortalWrt 固件。

- 目标设备：`cmcc_rax3000q`
- 内核：`5.4-QSDK`
- 构建方式：GitHub Actions
- 主要用途：旁路/主路由、海外分流、WireGuard、PassWall、SmartDNS、Tailscale/ZeroTier 组网

## 固件文件说明

每次成功构建后，Release 和 Actions Artifact 里会出现这些文件：

| 文件 | 用途 | 是否刷入系统 |
| --- | --- | --- |
| `*-squashfs-nand-factory.ubi` | NAND 机型正式 factory 固件，适合通过支持 UBI 的恢复模式、U-Boot 或 factory 刷机方式写入 | 是 |
| `*-squashfs-nand-factory.bin` | 与 `.ubi` 内容完全相同的副本，只是改成 `.bin` 后缀，方便某些网页上传框识别 | 是，但本质仍是 UBI 固件 |
| `*-initramfs-fit-uImage.itb` | 临时内存启动镜像，常用于 U-Boot/TFTP 测试、救援、临时进系统 | 否，不建议当作最终系统刷入 |
| `*.manifest` | 本次固件内置软件包清单 | 否 |
| `sha256sums` | 固件校验值 | 否 |
| `*.buildinfo` | 构建配置和版本信息 | 否 |

如果你是从 OpenWrt/ImmortalWrt 网页“系统升级”页面刷机，先确认页面接受该固件；不确定时先在 SSH 里做测试：

```sh
sysupgrade -T /tmp/xxx-squashfs-nand-factory.ubi
```

检测不通过就不要硬刷，改用对应的 factory、U-Boot、恢复模式刷法。

## 已内置能力

### 网络基础

- IPv6 支持
- Wi-Fi NSS
- NAT NSS / TurboACC
- FullCone NAT
- TProxy / ipset / dnsmasq-full
- LuCI 中文 Web 管理界面

### 海外分流和代理

- PassWall
- Xray Core `26.3.27`
- Shadowsocks-libev 客户端/服务端
- ShadowsocksR-libev 客户端
- Trojan Plus
- HAProxy
- Simple Obfs
- `v2ray-geoip`

说明：

- 已关闭旧版 `v2ray-core`、`v2ray-plugin` 和 `v2ray-geosite`，避免旧 Go / gVisor 依赖导致 Actions 编译失败。
- 目前推荐用 PassWall + Xray 节点做 VLESS/TLS/TCP 等分流。

### DNS

- SmartDNS
- ChinaDNS-NG
- dnsmasq-full，启用 DHCP、DHCPv6、DNSSEC、ipset、conntrack、TFTP 等能力

### VPN / 组网

- WireGuard 内核模块：`kmod-wireguard`
- WireGuard 工具：`wireguard-tools`
- LuCI WireGuard 管理界面：`luci-app-wireguard`
- LuCI WireGuard 协议支持：`luci-proto-wireguard`
- Tailscale `1.98.3`
- ZeroTier

### 常用 LuCI 插件和工具

- ttyd 网页终端
- 文件传输
- NATMap
- UPnP
- WOL 网络唤醒
- 自动重启
- Watchcat
- TurboACC
- vlmcsd
- xlnetacc

## 未内置或已关闭

为了保证 RAX3000Q/QY 能稳定编译并控制固件体积，以下能力没有内置：

- Docker / Dockerman
- OpenClash
- AdGuardHome
- mosdns
- ddns-go
- frpc
- 旧版 v2ray-core / v2ray-plugin / v2ray-geosite

## 刷机提醒

- `.bin` 文件只是 `.ubi` 的改名副本，不是另外一种 sysupgrade 格式。
- `.itb` 用于临时启动，不要当作最终固件刷入。
- 刷机前建议核对 `sha256sums`，并确认当前设备确实是 RAX3000Q / RAX3000QY。
- 如果当前系统、分区布局或刷机入口不确定，先查清楚再刷，避免写错分区。

## 来源

Base from [P3TERX/Actions-OpenWrt](https://github.com/P3TERX/Actions-OpenWrt).

Get SSH: [GetSSH](https://hugo.utermux.dev/default/rax3000q-latest/)

UBoot: [UBoot](https://github.com/hzyitc/openwrt-redmi-ax3000/issues/73#issuecomment-2259591683) Set computer IP to `192.168.1.8`, use LAN1 port.

## Acknowledgments

- [Microsoft](https://www.microsoft.com)
- [Microsoft Azure](https://azure.microsoft.com)
- [GitHub](https://github.com)
- [GitHub Actions](https://github.com/features/actions)
- [tmate](https://github.com/tmate-io/tmate)
- [mxschmitt/action-tmate](https://github.com/mxschmitt/action-tmate)
- [csexton/debugger-action](https://github.com/csexton/debugger-action)
- [Cisco](https://www.cisco.com/)
- [ImmortalWrt](https://github.com/kkstone/immortalwrt-ipq50xx)

## License

[MIT](https://github.com/P3TERX/Actions-OpenWrt/blob/main/LICENSE) © P3TERX
