# XMPlus Backend Network Settings

### Server Transport(Network) Settings 

## TCP

```json
{
  "transport" : "tcp",
  "acceptProxyProtocol": false,
  "flow" : "xtls-rprx-vision",
  "header": {
    "type": "none"
  }
}
```

## TCP + HTTP (obfuscation)

```json
{
  "transport" : "tcp",
  "acceptProxyProtocol": false,
  "header": {
    "type": "http",
    "request": {
      "path": "/xmplus",
      "headers": {
        "Host": "x.tld.com"
      }
    }
  }
}
```
> `acceptProxyProtocol:` true | false

It indicates whether to receive the PROXY protocol. PROXY protocolopen in new window is dedicated to passing the real source IP and port of the request, if you don’t understand it, please ignore

> `path`: Http Header path, The default value is /

> `Host`: Http Header host

> `flow`:  none | xtls-rprx-vision 

> Transport method based on tcp [TCP Method](https://xtls.github.io/Xray-docs-next/config/transports/tcp.html)


## WS

```json
{
  "transport" : "ws",
  "acceptProxyProtocol": false,
  "path": "/xmplus",
  "headers": {
    "Host": "x.tld.com"
  }
}
```
> `acceptProxyProtocol:` true | false

It indicates whether to receive the PROXY protocol. PROXY protocolopen in new window is dedicated to passing the real source IP and port of the request, if you don’t understand it, please ignore

> `path`: The HTTP protocol path used by WebSocket, the default value is "/".

> `Host`: Custom HTTP header host

> Transport method based on ws [WS Method](https://xtls.github.io/Xray-docs-next/config/transports/websocket.html)

## H2

```json
{
  "transport" : "h2",
  "acceptProxyProtocol": false,
  "host": "x.tld.com",
  "path": "/xmplus"
}
```
> `acceptProxyProtocol:` true | false

It indicates whether to receive the PROXY protocol. PROXY protocolopen in new window is dedicated to passing the real source IP and port of the request, if you don’t understand it, please ignore

> `path`: HTTP path, starting with /, client and server must be the same.

> `Host`: Custom HTTP header host

> Transport method based on ws [WS Method](https://xtls.github.io/Xray-docs-next/config/transports/h2.html)


## GRPC

```json
{
  "transport" : "grpc",
  "acceptProxyProtocol": false,
  "serviceName": "xmplus"
}
```

> `acceptProxyProtocol:` true | false

It indicates whether to receive the PROXY protocol. PROXY protocolopen in new window is dedicated to passing the real source IP and port of the request, if you don’t understand it, please ignore

> `serviceName`: behave like path in ws

> Transport method based on gRPC [GRPC Method](https://xtls.github.io/Xray-docs-next/config/transports/grpc.html)


## QUIC

backend quic transport failed to start run command: sysctl -w net.core.rmem_max=2500000 

Increase buffer size [quic udp problem](https://github.com/lucas-clemente/quic-go/wiki/UDP-Receive-Buffer-Size)

```json
{
  "transport" : "quic",
  "acceptProxyProtocol": false,
  "security": "none",
  "key": "",
  "header": {
    "type": "none"
  }
}
```

> `acceptProxyProtocol:` true | false

It indicates whether to receive the PROXY protocol. PROXY protocolopen in new window is dedicated to passing the real source IP and port of the request, if you don’t understand it, please ignore

> `security`: "none" | "aes-128-gcm" | "chacha20-poly1305"

> `header type`: "none" | "srtp" | "utp" | "wechat-video" | "dtls" | "wireguard"

> `key`: The key used for encryption

> Transport method based on quic [QUIC Method](https://xtls.github.io/Xray-docs-next/config/transports/quic.html)

## KCP

```json
{
  "transport" : "kcp",
  "acceptProxyProtocol": false,
  "congestion": false,
  "header": {
    "type": "none"
  },
  "seed": "password"
}
```
> `acceptProxyProtocol:` true | false

It indicates whether to receive the PROXY protocol. PROXY protocolopen in new window is dedicated to passing the real source IP and port of the request, if you don’t understand it, please ignore

> `congestion` true | false

Whether to enable congestion control. Xray will automatically monitor the network quality, and when the packet loss is serious, it will automatically reduce the throughput; when the network is smooth, it will also increase the throughput appropriately.

> `header type`: "none" | "srtp" | "utp" | "wechat-video" | "dtls" | "wireguard"

Masquerade type, optional

> `seed`: Optional obfuscation password, 

Uses AES-128-GCM algorithm to obfuscate traffic data, client and server need have same password.

> Transport method based on kcp [KCP Method](https://xtls.github.io/Xray-docs-next/config/transports/mkcp.html)
