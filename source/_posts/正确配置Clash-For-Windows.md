---
title: 正确配置Clash For Windows的TUN、TAP、系统代理三种模式？
tags:
  - 'clash'
categories:
  - [clash]
top_img: 
date: 2022-06-30 13:30:51
updated: 2022-06-30 13:30:51
cover:
description:
keywords:
---


# Clash的三种代理模式


因为有些软件不走系统代理，就算我们开启了翻墙，也不能让这些软件走翻墙。

## 1、系统代理模式
- 最简单的一种模式，只需要开启系统代理的开关即可。
- 但是有些软件不走系统代理，因此这种模式适合普通用户，平常用浏览器进行科学上网的。


## 2、Clash Tap Device模式

- Clash Tap 虚拟网卡模式，让所有的流量走虚拟网卡，实现真正的全局翻墙模式，实现所有软件代理。

## 3、Clash TUN Mode模式?

- 让电脑所有的流量都走虚拟网卡，实现全局代理，和TAP模式类似。


## 4、Clash TAP TUN 区别？

Clash TUN 和 TAP 类似，都是让所有流量走虚拟网卡，实现所有软件翻墙，Windows下TUN 模式性能比TAP模式好，推荐使用TUN模式。


# 什么情况下需要开启TAP/TUN ？

在使用一些游戏软件需要加速，或者一些不遵循系统代理的软件时，需要开启 TAP/TUN 模式。

- 专业用户，例如IT、开发人员，需要经常操作终端，使用开发软件，用到Linux子系统的建议使用TUN模式。
- 其实大部分软件都走系统代理，例如浏览器，但是有些不走系统代理，例如某些游戏，Git等。平时使用浏览器翻墙，比如访问google网站，或者使用一些遵循系统代理的软件时，不需要开启 TAP/TUN 模式。建议直接使用系统代理模式。


# 开启TUN全局代理模式

![](/img/cut/clash-1.webp)

按照上面三步依次执行，最后点开`mixin`选项，编辑文件，删除里面的内容，粘贴上下面的内容即可！
```yml
mixin:
   hosts:
     'mtalk.google.com': 108.177.125.188
     'services.googleapis.cn': 74.125.203.94
     'raw.githubusercontent.com': 151.101.76.133
   dns:
     enable: true
     default-nameserver:
       - 223.5.5.5   # 阿里的DNS服务器
       - 1.0.0.1     # CloudFlare的DNS服务器
     ipv6: false
     enhanced-mode: redir-host #fake-ip
     nameserver:
       - https://dns.rubyfish.cn/dns-query
       - https://223.5.5.5/dns-query
       - https://dns.pub/dns-query
     fallback:
       - https://1.0.0.1/dns-query
       - https://public.dns.iij.jp/dns-query
       - https://dns.twnic.tw/dns-query
     fallback-filter:
       geoip: true
       ipcidr:
         - 240.0.0.0/4
         - 0.0.0.0/32
         - 127.0.0.1/32
       domain:
         - +.google.com
         - +.facebook.com
         - +.twitter.com
         - +.youtube.com
         - +.xn--ngstr-lra8j.com
         - +.google.cn
         - +.googleapis.cn
         - +.gvt1.com
   tun:
     enable: true
     stack: gvisor
     dns-hijack:
       - 198.18.0.2:53
     macOS-auto-route: true
     macOS-auto-detect-interface: true # 自动检测出口网卡
```

# 一些小问题

- 三种代理模式开启一种即可。
  - 不要开关全点开，可能会有冲突。
  - 例如开启了TUN模式，就不要再将System Proxoy（系统代理）的开关点开了。
