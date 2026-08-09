# NodeX 路由器版（LuCI）

NodeX 节点管家——**OpenWrt 路由器版本发布仓库**。安装后出现在 LuCI「服务」菜单中，管理 Xray/Hysteria2 节点、对接 Xboard 面板、支持 XrayR 转发模式。

> 开发源码见 [wooxi/nodex](https://github.com/wooxi/nodex)（主仓库）；本仓库只发布路由器安装包。

## 📦 安装包说明

最新版本：**[v0.3.2](https://github.com/wooxi/nodex-luci/releases/latest)**（[全部版本](https://github.com/wooxi/nodex-luci/releases)）

| 文件 | 说明 | 下载 |
|---|---|---|
| `nodex_*.ipk` | **守护进程包**：nodex 单二进制 + init.d 开机自启 + 默认配置 | [下载](https://github.com/wooxi/nodex-luci/releases/latest/download/nodex_0.3.0-1_x86_64.ipk) |
| `luci-app-nodex_*.ipk` | **LuCI 界面包**：服务菜单集成，原生 LuCI 风格 UI | [下载](https://github.com/wooxi/nodex-luci/releases/latest/download/luci-app-nodex_0.3.0-1_all.ipk) |
| `nodex-linux-amd64` | 守护进程裸二进制（手动部署用，免 ipk） | [下载](https://github.com/wooxi/nodex-luci/releases/latest/download/nodex-linux-amd64) |
| `xray-linux-amd64` | **Xray 内核**（VLESS/VMess/Trojan/SS） | [下载](https://github.com/wooxi/nodex-luci/releases/latest/download/xray-linux-amd64) |
| `hysteria-linux-amd64` | **Hysteria2 内核** | [下载](https://github.com/wooxi/nodex-luci/releases/latest/download/hysteria-linux-amd64) |

## 🚀 安装（命令行）

```bash
# 1. 安装内核（x86_64 架构）
wget -O /usr/bin/xray https://github.com/wooxi/nodex-luci/releases/latest/download/xray-linux-amd64
wget -O /usr/bin/hysteria https://github.com/wooxi/nodex-luci/releases/latest/download/hysteria-linux-amd64
chmod +x /usr/bin/xray /usr/bin/hysteria

# 2. 安装 ipk
wget -O /tmp/nodex.ipk https://github.com/wooxi/nodex-luci/releases/latest/download/nodex_0.3.0-1_x86_64.ipk
wget -O /tmp/luci-app-nodex.ipk https://github.com/wooxi/nodex-luci/releases/latest/download/luci-app-nodex_0.3.0-1_all.ipk
opkg install /tmp/nodex.ipk /tmp/luci-app-nodex.ipk

# 3. 启动（或 LuCI → 服务 → NodeX）
/etc/init.d/nodex enable
/etc/init.d/nodex start
```

> 💡 **也可以完全在 LuCI 界面内完成**：NodeX → 总览 → 后端状态 → 「下载并安装后端」（自动拉取 nodex + 双内核）。

## ✨ 功能

- 多节点管理（每节点独立 xray/hysteria2 进程，独立端口/API/同步）
- Xboard 面板对接（UniProxy 协议：拉用户 / 推流量 / 心跳 / 状态）
- 协议支持：vless(+Reality/TLS/WS) / vmess / trojan / shadowsocks / hysteria2
- 转发出站（XrayR 转发模式，多目标负载均衡）
- 内核总开关、核心一键下载/更新、流量统计、在线用户/IP
- 内核崩溃自动拉起（看门狗）

## 🛠 面板配置步骤

1. Xboard 后台 → 服务器 → 添加节点（v2ray 或 hysteria），记录**节点 ID**
2. Xboard 后台 → 系统设置 → 获取**通信密钥**（server_token）
3. LuCI → NodeX → 面板对接 → 填入面板地址 / 密钥
4. LuCI → NodeX → 节点管理 → 新增节点 → 填节点 ID、选协议
5. LuCI → NodeX → 总览 → 启动

## 📋 目录结构

```
openwrt/          nodex 守护进程包（二进制 + init.d + 默认配置）
openwrt-luci/     luci-app-nodex 界面包（controller + 模板 + JS）
.github/workflows/  CI：编译 ipk + 附内核二进制，打 tag 自动发布
```
