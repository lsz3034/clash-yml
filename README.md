## 推荐大家使用 [sing-box](https://github.com/233boy/sing-box)

最好用的 sing-box 一键安装脚本 & 管理脚本 

帮助您快速的搭建自己的vps节点

## [使用纯前端页面,将 vmess 节点链接 转化为 Clash 配置文件 ](https://lsz3034.github.io/clash-yml/qrcode/2.html)

## [使用纯前端页面,将 vmess 节点链接 转化为二维码 供你其他设备分享使用](https://lsz3034.github.io/clash-yml/qrcode/1.html)

# Clash 配置 Vmess 节点指南

本指南将帮助您通过配置 Clash 的 `config.yml` 文件连接到 Vmess 节点。以下是具体的步骤和配置示例。

## 1. 配置基础信息

`config.yml` 文件是 Clash 的核心配置文件。确保包含以下基础信息：

```yaml
mixed-port: 7890
allow-lan: true
mode: rule
log-level: info
external-controller: '0.0.0.0:9090'
secret: ''
```

## 2. 配置 DNS

启用 DNS 并配置相关参数以优化网络性能：

```yaml
dns:
  enable: true
  ipv6: false
  listen: '0.0.0.0:5353'
  enhanced-mode: fake-ip
  fake-ip-filter:
    - '*.lan'
    - localhost.ptlogin2.qq.com
  nameserver:
    - 'https://223.5.5.5/dns-query'
    - 'https://doh.pub/dns-query'
  fallback:
    - 'https://162.159.36.1/dns-query'
    - 'https://dns.google/dns-query'
    - 'tls://8.8.8.8:853'
  fallback-filter:
    geoip: false
    ipcidr:
      - 0.0.0.0/8
      - 10.0.0.0/8
      - 100.64.0.0/10
      - 127.0.0.0/8
      - 169.254.0.0/16
      - 172.16.0.0/12
      - 192.0.0.0/24
      - 192.0.2.0/24
      - 192.88.99.0/24
      - 192.168.0.0/16
      - 198.18.0.0/15
      - 198.51.100.0/24
      - 203.0.113.0/24
      - 224.0.0.0/4
      - 240.0.0.0/4
      - 255.255.255.255/32
    domain:
      - +.google.com
      - +.facebook.com
      - +.youtube.com
      - +.githubusercontent.com
```

## 3. 添加 Vmess 节点

```yaml
proxies:
  - name: "example_proxy"
    type: vmess
    server: <your_server>
    port: <your_port>
    uuid: <your_uuid>
    alterId: 0
    cipher: auto
    tls: true
    skip-cert-verify: false
    network: ws
    ws-opts:
      path: "<your_path>"
      headers:
        Host: <your_host>

proxy-groups:
  - name: "代理"
    type: select
    proxies:
      - "example_proxy"

rules:
  - MATCH,代理
```

**说明：**

- `<your_server>`：替换为服务器地址。
- `<your_port>`：替换为端口号。
- `<your_uuid>`：替换为用户 ID。
- `<your_path>`：替换为 WebSocket 路径。
- `<your_host>`：替换为伪装域名。



## 4. 配置代理组

将添加的 Vmess 节点包含到代理组中，方便规则选择：

```yaml
proxy-groups:
  - name: "代理"
    type: select
    proxies:
      - "example_proxy"
```

## 5. 设置规则

根据需求定义规则，以便流量自动选择代理或直连：

```yaml
rules:
  - MATCH,代理
```

## 6. 启动 Clash

完成配置后，保存 `config.yml` 文件并启动 Clash 客户端。如果一切正常，Clash 将自动连接到 Vmess 节点。

---

至此，您已成功设置 Clash 连接 Vmess 节点。根据需求可以随时调整配置。





