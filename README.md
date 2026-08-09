# NodeX 路由器版（LuCI）

NodeX 节点管家——**OpenWrt 路由器版本发布仓库**。安装后出现在 LuCI「服务」菜单中，管理 Xray/Hysteria2 节点、对接 Xboard 面板、支持 XrayR 转发模式。

> 开发源码见 [wooxi/nodex](https://github.com/wooxi/nodex)（主仓库）；本仓库只发布路由器安装包。

## 安装

```bash
# 1. 下载 release 中的 ipk 与内核二进制
#    nodex_*.ipk          — 守护进程
#    luci-app-nodex_*.ipk — LuCI 界面
#    xray-linux-amd64     — Xray 内核
#    hysteria-linux-amd64 — Hysteria2 内核

# 2. 安装内核到 /usr/bin/
cp xray-linux-amd64 /usr/bin/xray
cp hysteria-linux-amd64 /usr/bin/hysteria
chmod +x /usr/bin/xray /usr/bin/hysteria

# 3. 安装 ipk
opkg install nodex_*.ipk luci-app-nodex_*.ipk

# 4. 启动（LuCI 服务菜单 → NodeX）
/etc/init.d/nodex enable
/etc/init.d/nodex start
```

**也可在 LuCI 界面内一键下载安装**：NodeX → 总览 → 后端状态 → 「下载并安装后端」。

## 功能

- 多节点管理（每节点独立 xray/hysteria2 进程）
- Xboard 面板对接（UniProxy 协议：拉用户 / 推流量 / 心跳 / 状态）
- 协议：vless(+Reality/TLS/WS) / vmess / trojan / shadowsocks / hysteria2
- 转发出站（XrayR 转发模式，多目标负载均衡）
- 内核总开关、核心一键下载/更新、在线用户/IP、流量统计
- 内核崩溃自动拉起（看门狗）

## 版本

| 组件 | 版本 |
|---|---|
| nodex 守护进程 | v0.3.2 |
| luci-app-nodex | v0.3.2 |
| xray | 最新 release |
| hysteria | 最新 release |
