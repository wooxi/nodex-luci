# NodeX · Router

OpenWrt packages for NodeX. Once installed, NodeX appears under **Services → NodeX** in LuCI and manages Xray / Hysteria 2 nodes against a panel.

[中文](README.md) · [Source](https://github.com/wooxi/nodex) · [Container build](https://github.com/wooxi/nodex-docker) · [Control panel](https://github.com/wooxi/xpanel)

---

## About

This repository only packages. It carries no Go source. Each build pulls from [wooxi/nodex](https://github.com/wooxi/nodex), compiles the daemon, and bundles it with the init script, default configuration and LuCI UI into ipk packages.

| Package | Contents |
|---|---|
| `nodex_*.ipk` | daemon — single binary, `/etc/init.d/nodex`, default config |
| `luci-app-nodex_*.ipk` | LuCI UI — service menu entry, forms, log viewer |

The release also ships `nodex-linux-amd64`, `xray-linux-amd64` and `hysteria-linux-amd64`. The UI uses them for one-click backend install and kernel updates; they can also be deployed by hand.

Protocols, node management and panel integration are documented in the [main repository](https://github.com/wooxi/nodex). This repository covers what is specific to OpenWrt.

## Installation

### Through LuCI

1. Install `luci-app-nodex_*.ipk`
2. Services → NodeX → Overview → Backend → *Download and install backend*

That pulls the matching daemon and both kernels — no shell required.

### From the command line

```bash
# kernels
wget -O /usr/bin/xray      https://github.com/wooxi/nodex-luci/releases/latest/download/xray-linux-amd64
wget -O /usr/bin/hysteria  https://github.com/wooxi/nodex-luci/releases/latest/download/hysteria-linux-amd64
chmod +x /usr/bin/xray /usr/bin/hysteria

# packages
wget -O /tmp/nodex.ipk           https://github.com/wooxi/nodex-luci/releases/latest/download/nodex_amd64.ipk
wget -O /tmp/luci-app-nodex.ipk  https://github.com/wooxi/nodex-luci/releases/latest/download/luci-app-nodex_all.ipk
opkg install /tmp/nodex.ipk /tmp/luci-app-nodex.ipk

/etc/init.d/nodex enable
/etc/init.d/nodex start
```

Actual filenames carry architecture and version suffixes; take them from the [latest release](https://github.com/wooxi/nodex-luci/releases/latest).

## Connecting a panel

1. Panel → Servers → add a node (v2ray or hysteria), note the **node ID**
2. Panel → Settings → copy the **communication key**
3. LuCI → NodeX → Panel: enter panel URL and key
4. LuCI → NodeX → Nodes → add: enter node ID, pick protocol
5. LuCI → NodeX → Overview → Start

The panel can be [XPanel](https://github.com/wooxi/xpanel), Xboard, V2Board or any UniProxy-compatible implementation.

## Requirements

| Item | Requirement |
|---|---|
| Architecture | x86_64 (current artifacts) |
| OpenWrt | 23.05 or later, `luci-base` |
| Memory | ~20 MB resident, kernels excluded |

## Building

Pushing a tag runs GitHub Actions: pull nodex source → build frontend and binary → package with OpenWrt SDK → attach kernel binaries → publish release.

New commits upstream trigger a build through `repository_dispatch`, with the version bumped automatically.

## License

MIT — see [LICENSE](LICENSE).
