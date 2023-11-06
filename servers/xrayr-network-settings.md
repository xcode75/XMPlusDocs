# XrayR Backend Network Settings

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

## H2

```json
{
  "transport" : "h2",
  "acceptProxyProtocol": false,
  "host": "x.tld.com",
  "path": "/xmplus"
}
```


## GRPC

```json
{
  "transport" : "grpc",
  "acceptProxyProtocol": false,
  "serviceName": "xmplus"
}
```


## QUIC

backend quic transport failed to start run command: sysctl -w net.core.rmem_max=2500000 

Increase buffer size [quic udp problem](https://github.com/lucas-clemente/quic-go/wiki/UDP-Receive-Buffer-Size)

```json
{
  "transport" : "quic",
  "acceptProxyProtocol": false,
  "header": {
    "type": "none"
  }
}
```



## KCP

```json
{
  "transport" : "kcp",
  "acceptProxyProtocol": false,
  "header": {
    "type": "none"
  }
}
```

