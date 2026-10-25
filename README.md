# NodeX · Router

NodeX 的 OpenWrt 安装包。装好后出现在 LuCI 的「服务 → NodeX」菜单里，用于管理 Xray / Hysteria 2 节点并对接面板。

[English](README.en.md) · [源码仓库](https://github.com/wooxi/nodex) · [容器版](https://github.com/wooxi/nodex-docker) · [控制面板](https://github.com/wooxi/xpanel)

---

## 说明

本仓库只做打包，不含 Go 源码。每次构建从 [wooxi/nodex](https://github.com/wooxi/nodex) 拉取源码编译守护进程，再与 init 脚本、默认配置和 LuCI 界面一起打成 ipk。

| 包 | 内容 |
|---|---|
| `nodex_*.ipk` | 守护进程：单二进制 + `/etc/init.d/nodex` + 默认配置 |
| `luci-app-nodex_*.ipk` | LuCI 界面：服务菜单入口、表单、日志查看 |

发布产物中的 `nodex-linux-amd64`、`xray-linux-amd64`、`hysteria-linux-amd64` 供界面内的一键安装与内核更新使用，也可单独下载手动部署。

功能、协议支持与面板对接说明见 [主仓库](https://github.com/wooxi/nodex)。本仓库只描述路由器特有的安装方式。

## 安装

### 通过 LuCI

1. 安装 `luci-app-nodex_*.ipk`
2. 服务 → NodeX → 总览 → 后端状态 → 「下载并安装后端」

该入口会拉取与本版本配套的守护进程与两个内核，无需命令行操作。

### 通过命令行

```bash
# 内核
wget -O /usr/bin/xray      https://github.com/wooxi/nodex-luci/releases/latest/download/xray-linux-amd64
wget -O /usr/bin/hysteria  https://github.com/wooxi/nodex-luci/releases/latest/download/hysteria-linux-amd64
chmod +x /usr/bin/xray /usr/bin/hysteria

# 安装包
wget -O /tmp/nodex.ipk           https://github.com/wooxi/nodex-luci/releases/latest/download/nodex_amd64.ipk
wget -O /tmp/luci-app-nodex.ipk  https://github.com/wooxi/nodex-luci/releases/latest/download/luci-app-nodex_all.ipk
opkg install /tmp/nodex.ipk /tmp/luci-app-nodex.ipk

/etc/init.d/nodex enable
/etc/init.d/nodex start
```

具体的文件名带架构与版本后缀，请以 [Releases](https://github.com/wooxi/nodex-luci/releases/latest) 页面为准。

## 对接面板

1. 面板 → 服务器 → 添加节点（v2ray 或 hysteria），记下**节点 ID**
2. 面板 → 系统设置 → 获取**通信密钥**
3. LuCI → NodeX → 面板对接：填面板地址与密钥
4. LuCI → NodeX → 节点管理 → 新增节点：填节点 ID，选协议
5. LuCI → NodeX → 总览 → 启动

面板可以是 [XPanel](https://github.com/wooxi/xpanel)、Xboard、V2Board 或任何兼容 UniProxy 的实现。

## 兼容性

| 项 | 要求 |
|---|---|
| 架构 | x86_64（当前发布产物） |
| OpenWrt | 23.05 及以上，需 `luci-base` |
| 内存 | 常驻约 20 MB，不含内核 |

## 构建与发布

打 tag 触发 GitHub Actions，构建流程为：拉取 nodex 源码 → 编译前端与二进制 → OpenWrt SDK 打包 → 附上内核二进制 → 发布 Release。

上游 nodex 有新提交时会通过 `repository_dispatch` 自动触发一次构建，版本号自动递增。

## 许可

本项目采用 MIT 许可证，见 [LICENSE](LICENSE)。
