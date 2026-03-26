---
layout: post
title: "在 ClawCloud 上部署 xray-reality"
date: 2026-03-25
categories: [技术]
tags: [xray, ClawCloud]
---

## 目的
最小化成本实现稳定的科学上网

## 正文
1.需要VPS,推荐薅羊毛，ClawCloud注册，github 账号满 180 天即可领每月 5 刀额度。
ClawCloud爪云首页，https://run.claw.cloud

![注册页面]({{ '/assets/images/ClawCloud_sign.png' | relative_url }}) <!-- 自适应大小 -->

2.创建 xray-reality 容器

可选 US West（singapore/Japan/Gemany/US East） -> APP Launch -> Create App  

Application Name: xray-reality  

Image(选 Public): ghcr.io/xtls/xray-core:latest  

CPU/Memory: 0.1/64M (最小即可)  

network: Public access 打开选 tcp  

Configmaps:  

filename: /usr/local/etc/xray/config.json  

File content:
```
{
  "log": {
    "loglevel": "warning"
  },
  "inbounds": [
    {
      "port": 8443,
      "protocol": "vless",
      "settings": {
        "clients": [
          {
            "id": "",  // run `xray uuid` to generate
            "flow": "xtls-rprx-vision"
          }
        ],
        "decryption": "none"
      },
      "streamSettings": {
        "network": "tcp",
        "security": "reality",
        "realitySettings": {
          "dest": "www.cloudflare.com:443",  // A website that support TLS1.3 and h2. You can also use `1.1.1.1:443` as dest
          "serverNames": [
            "www.cloudflare.com"            // A server name in the cert of dest site. 
          ],
          "privateKey": "", // run `xray x25519` to generate. Public and private keys need to be corresponding.
          "shortIds": [
            "" // 0 to f, length is a multiple of 2, maximum length is 16
          ]
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "tag": "direct"
    }
  ],
  "dns": {
    "servers": [
      "1.1.1.1",
      "8.8.8.8",
      "223.5.5.5"
    ]
  }
}
```

![总览页面]({{ '/assets/images/xray_overview.png' | relative_url }}) <!-- 自适应大小 -->

3.客户端配置
windows/android 下需安装 v2rayNG，https://github.com/2dust/v2rayN/releases

在 v2rayN 中，添加节点时填写：
vless://xxx(uuid)@xxx?encryption=none&flow=xtls-rprx-vision&security=reality&sni=www.cloudflare.com&fp=chrome&pbk=xxx(Public key)&sid=xxx&type=tcp&headerType=none#ClawCloud

参考：Docker Compose 部署 Xray 代理服务器 https://blog.leoho.dev/posts/docker-compose-deploy-xray/
