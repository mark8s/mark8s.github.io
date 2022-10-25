---
layout: post
title: HTTP 请求头中的 X-Forwarded-For
subtitle:
categories: http
tags: [http]
---

# 概述
这两天工作中刚好用到了 X-Forwarded-For，所以记录一下。看来上一次发布博客的记录。。。时间真快啊。。

在生产环境中，客户端请求通常不会与服务端直连，中间往往经过若干代理、负载均衡服务器，所以基于 TCP 层面拿到的 remote_address 将是这个链路中离服务端最近设备的 IP（这个 IP 无法伪造，否则三次握手无法成功）。所以需要有一种方式能获取到客户端真实 IP。

X-Forwarded-For 的出现便是为了解决这一问题，这个扩展头部不是HTTP/1.1（RFC2616）定义的。在 RFC7239 中，推出了 Forwarded 用于替换已经成为既定标准的前者。

## X-Forwarded-For 请求头格式

```shell
X-Forwarded-For: client, proxy1, proxy2
```

## X-Forwarded-For和相关几个头部的理解

- $remote_addr： 
是nginx与客户端进行TCP连接过程中，获得的客户端真实地址. Remote Address 无法伪造，因为建立 TCP 连接需要三次握手，如果伪造了源 IP，无法建立 TCP 连接，更不会有后面的 HTTP 请求

- X-Real-IP： 
是一个自定义头。X-Real-Ip 通常被 HTTP 代理用来表示与它产生 TCP 连接的设备 IP，这个设备可能是其他代理，也可能是真正的请求端。需要注意的是，X-Real-Ip 目前并不属于任何标准，代理和 Web 应用之间可以约定用任何自定义头来传递这个信息

- X-Forwarded-For：
X-Forwarded-For 是一个扩展头。HTTP/1.1（RFC 2616）协议并没有对它的定义，它最开始是由 Squid 这个缓存代理软件引入，用来表示 HTTP 请求端真实 IP，现在已经成为事实上的标准，被各大 HTTP 代理、负载均衡等转发服务广泛使用，并被写入 RFC 7239（Forwarded HTTP Extension）标准之中.

如果一个 HTTP 请求到达服务器之前，经过了三个代理 Proxy1、Proxy2、Proxy3，IP 分别为 IP1、IP2、IP3，用户真实 IP 为 IP0，那么按照 XFF 标准，服务端最终会收到以下信息：

```shell
X-Forwarded-For: IP0, IP1, IP2
```

Proxy3 直连服务器，它会给 XFF 追加 IP2，表示它是在帮 Proxy2 转发请求。列表中并没有 IP3，IP3 可以在服务端通过 remote_address 字段获得。我们知道 HTTP 连接基于 TCP 连接，HTTP 协议中没有 IP 的概念，remote_address 来自 TCP 连接，表示与服务端建立 TCP 连接的设备 IP，在这个例子里就是 IP3。

详细分析一下，这样的结果是经过这样的流程而形成的：

1.用户 IP0---> 代理 Proxy1（IP1），Proxy1记录用户IP0，并将请求转发个Proxy2时，带上一个Http Header
`X-Forwarded-For: IP0`

2.Proxy2收到请求后读取到请求有 X-Forwarded-For: IP0，然后proxy2 继续把链接上来的proxy1 ip追加到 X-Forwarded-For 上面，构造出`X-Forwarded-For: IP0, IP1`，继续转发请求给Proxy 3

3.同理，Proxy3 按照第二部构造出 X-Forwarded-For: IP0, IP1, IP2,转发给真正的服务器，比如NGINX，nginx收到了http请求，里面就是 `X-Forwarded-For: IP0, IP1, IP2` 这样的结果。所以Proxy 3 的IP3，不会出现在这里。

4.nginx 获取proxy3的IP 能通过`remote_address`获取到，因为这个remote_address就是真正建立TCP链接的IP，这个不能伪造，是直接产生链接的IP。`$remote_address` 无法伪造，因为建立 TCP 连接需要三次握手，如果伪造了源 IP，无法建立 TCP 连接，更不会有后面的 HTTP 请求。

## References

[HTTP 请求头中的 X-Forwarded-For](https://www.cnblogs.com/powerwu/articles/15975117.html)
