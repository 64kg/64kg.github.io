---
title: Bypassing GFW
date: 2021-08-15 15:07:00
updated: 2021-09-05 20:06:00
categories:
- Tech
tags:
- Software
---

Continiously recording methods, tools and scripts for the purpose of bypass GFW.

<!-- more -->

# VPN

## OpenVPN

[OpenVPN install script](/attach/openvpn.sh)

# Proxy

## SS/SSR

[SSR install script](/attach/ssr.sh)

## Clash

[~~Kr328/clash-tun-for-linux~~](https://github.com/Kr328/clash-tun-for-linux)

👆 not stable

### Ubuntu

Download premium release from [Dreamacro/clash/releases/tag/premium](https://github.com/Dreamacro/clash/releases/tag/premium).

[边缘@订阅转换API](https://bianyuan.xyz/)

[Clash web ui](https://github.com/haishanh/yacd)

Systemd service file: (add the `ExecStart` below, copy to `/usr/lib/systemd/system` and enable it)
```
[Unit]
Description=A rule based proxy tunnel with tun support
After=network-online.target network.target

[Service]
Type=simple
User=nobody
CapabilityBoundingSet=CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_BIND_SERVICE
ExecStart=

[Install]
WantedBy=multi-user.target
```

Common configs:
```yaml
port: 7890
socks-port: 7891
mode: rule
log-level: info
allow-lan: true
external-controller: :9090
external-ui: ../ui
```

