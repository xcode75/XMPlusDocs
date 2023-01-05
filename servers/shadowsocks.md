# Shadowsocks(2022)

| Protocol | Supported Transport |
| :--- | :--- |
| Shadowsocks | tcp, ws, grpc, quic |

Server port and listening port must be the same, unless you are using other method such as nginx(fill nginx port for server port) to process tls.

### TLS Configuration (Security Settings)

Check documentation [TLS Object](https://xtls.github.io/Xray-docs-next/config/transport.html#tlsobject)

> `serverName`: your certificate domain address

> `fingerprint`: "" | "chrome" | "firefox" | "safari" | "randomized"

```json
{
  "serverName": "xmplus.dev",
  "rejectUnknownSni": true,
  "allowInsecure": false,
  "fingerprint": "chrome"
}
```

## Server Transport Configuration (Network Settings)

- Remember to select the correct network setting for the transport type (Network)


## tcp

```json
{
  "acceptProxyProtocol": false,
  "header": {
    "type": "none"
  }
}
```

## tcp + http

default path  /
> `path`: your path

> `Host`: your host
 
```json
{
  "acceptProxyProtocol": false,
  "header": {
    "type": "http",
    "request": {
	  "path": "/",
      "headers": {
        "Host": "xmplus.dev"
      }
    }
  }
}
```

> Transport method based on tcp [TCP Method](https://xtls.github.io/Xray-docs-next/config/transports/tcp.html)


## ws(tls) 

default path  /
> `path`: your path

> `Host`: your host
 
```json
{
  "acceptProxyProtocol": false,
  "path": "/",
  "headers": {
    "Host": "xray.com"
  }
}
```

> Transport method based on ws [WS Method](https://xtls.github.io/Xray-docs-next/config/transports/websocket.html)


## grpc(tls)

> `serviceName`: behave like path in ws

```json
{
  "acceptProxyProtocol": false,
  "serviceName": "xray.com"
}
```

> Transport method based on gRPC [GRPC Method](https://xtls.github.io/Xray-docs-next/config/transports/grpc.html)


## quic + tls

> `security`: "none" | "aes-128-gcm" | "chacha20-poly1305"

> `header type`: "none" | "srtp" | "utp" | "wechat-video" | "dtls" | "wireguard"

backend quic transport failed to start run command: sysctl -w net.core.rmem_max=2500000 

> Increase buffer size [quic udp problem](https://github.com/lucas-clemente/quic-go/wiki/UDP-Receive-Buffer-Size)

```json
{
  "acceptProxyProtocol": false,
  "security": "none",
  "key": "",
  "header": {
    "type": "none"
  }
}
```

> Transport method based on quic [QUIC Method](https://xtls.github.io/Xray-docs-next/config/transports/quic.html)