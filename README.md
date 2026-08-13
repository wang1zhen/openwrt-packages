# OpenWrt personal packages

这是一个面向 OpenWrt `main` 的个人软件包仓库，目标设备为带有
`eth0`、`eth1`、`eth2`、`eth3` 四个网口的 x86/64 设备。

仓库中的 LuCI 应用最初来自 ImmortalWrt。本仓库只保留 OpenWrt 官方
LuCI 尚未提供、且个人镜像仍需要的部分，并将它们移植到现代 JavaScript、
ucode、rpcd ACL 和 procd 接口。

## 软件包

- `default-settings`：x86/64 四口设备的个人首次启动设置。
- `default-settings-chn`：简体中文、Asia/Shanghai 和中国大陆 NTP 设置。
- `luci-app-arpbind`：静态 ARP 绑定。
- `luci-app-autoreboot`：计划重启。
- `luci-app-ramfree`：手动释放页缓存。
- `vlmcsd` 与 `luci-app-vlmcsd`：vlmcsd 服务及其 LuCI 页面。

Web 服务器设置请使用 OpenWrt 官方的 `luci-app-uhttpd`。本仓库不再维护
旧的 `luci-app-webadmin` 或 `luci-app-zerotier`；ZeroTier 本体应直接使用
OpenWrt packages feed 中的 `zerotier`。

## 首次启动行为

`default-settings` 只允许在 `TARGET_x86_64` 上选择，并在运行时再次检查
`DISTRIB_TARGET=x86/64`。它会：

- 将 `eth0` 至 `eth3` 加入 `br-lan`；四个网口未全部出现时保留 OpenWrt
  自动生成的网络配置并写入系统日志。
- 将 LAN 设置为 DHCPv4/DHCPv6 客户端，删除 WAN/WAN6，并关闭 LAN 上的
  DHCPv4、DHCPv6、RA 和 NDP 服务。
- 在 HTTP 80 上接收请求并重定向到 HTTPS 8888；自签名证书由
  `luci-ssl`/uHTTPd 的官方机制生成。
- 使用官方 Bootstrap 主题和 OpenWrt 官方 APK 软件源。
- 安装仓库中指定的 root 密码哈希与 Dropbear SSH 公钥。

> [!WARNING]
> root 密码哈希和 SSH 公钥位于公开仓库中，不能视为秘密。这一行为只适合
> 受控的个人镜像；如果镜像会分发给他人，应在构建前替换或删除凭据。

本仓库不提供旧 UCI 配置迁移，更新时应生成全新镜像并不保留旧配置。

## 接入 OpenWrt buildroot

可以将仓库作为自定义 feed 使用，或链接到 OpenWrt 源码树的 `package/`
目录。作为 feed 时，在 `feeds.conf.default` 中添加：

```text
src-git personal https://github.com/wang1zhen/openwrt-packages.git
```

然后运行：

```sh
./scripts/feeds update personal
./scripts/feeds install -a -p personal
```

本仓库的顶层 `luci.mk` 来自 OpenWrt LuCI；各 LuCI 包使用 `../luci.mk`，
因此也可以直接把这些包目录复制或链接到同一父目录下构建。

## 上游同步基线

最后一次完整审计日期为 2026-08-13：

- OpenWrt `main`: `9f3157aa2ad5481de947bbfbba3e2eb065486a26`
- OpenWrt LuCI `master`: `31748dcc2aea830ab360e88448a29906596fef64`
- OpenWrt packages `master`: `95875218de25e6340251910a8c996e5a811c69b8`
- ImmortalWrt LuCI `master`: `39dca4a5b097178fb69b1243b19e58ca1d6afefe`
- ImmortalWrt packages `master`: `7300d4652494de24dcc9516a6668434855bc21a1`

LuCI 应用代码主要从对应的 ImmortalWrt 应用目录移植；运行时接口、
`luci.mk`、uHTTPd、APK 和 ZeroTier 兼容性以 OpenWrt main 为准。
