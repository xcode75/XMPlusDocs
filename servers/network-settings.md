## XMPlus Panel Server configuration

### Network Settings

#### TCP
```
{
  "transport" : "tcp",
  "acceptProxyProtocol": false,
  "flow": "xtls-rprx-vision",
  "header": {
    "type": "none"
  }
}
```
#### TCP + HTTP
```
{
  "transport" : "tcp",
  "acceptProxyProtocol": false,
  "header": {
    "type": "http",
    "request": {
      "path": "/xmplus",
      "headers": {
        "Host": ["www.baidu.com", "www.taobao.com", "www.cloudflare.com"]
      }
    }
  }
}
```
####  WS
```
{
  "transport": "ws",
  "acceptProxyProtocol": false,
  "path": "/xmplus?ed=2560",
  "host": "hk1.xyz.com",
  "cdn_host": "www.cloudflare.com"
}
```

####  H2
```
{
  "transport" : "h2",
  "acceptProxyProtocol": false,
  "host": "hk1.xyz.com",
  "path": "/",
  "cdn_host": "www.cloudflare.com"
}
```

####  GRPC
```
{
  "transport" : "grpc",
  "acceptProxyProtocol": false,
  "serviceName": "xmplus",
  "authority": "hk1.xyz.com"
}
```

####  HTTPUPGRADE
```
{
  "transport" : "httpupgrade",
  "acceptProxyProtocol": false,
  "host": "hk1.xyz.com",
  "path": "/xmplus?ed=2560",
  "cdn_host": "www.cloudflare.com"
}
```

####  SPLITHTTP
```
{
  "transport" : "splithttp",
  "host": "hk1.xyz.com",
  "cdn_host": "www.cloudflare.com",
  "path": "/",
  "scMaxEachPostBytes": 1000000,
  "scMaxConcurrentPosts": 100,
  "scMinPostsIntervalMs": 30,
  "noSSEHeader": false
}
```

####  QUIC
```
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
####  KCP
```
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
