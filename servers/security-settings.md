# Security Settings

NB: Server port and listening port must be the same, unless you are using other method such as nginx(fill nginx port for server port) to process tls.

### TLS Settings 

```json
{
  "serverName": "xmplus.dev",
  "rejectUnknownSni": true,
  "allowInsecure": false,
  "fingerprint": "chrome",
  "sni": "xmplus.dev",
  "flow": "none"
}
```

> `serverName`: your certificate domain address

Specifies the domain name of the server-side certificate, useful when the connection is established by IP.

> `fingerprint`: "" | "chrome" | "firefox" | "safari" | "randomized"

This parameter is used to configure the fingerprint of the specified TLS Client Hello. When its value is empty, it means that this function is not enabled. When enabled, Xray will simulate a TLS fingerprint via the uTLS library, or generate it randomly.

> `allowInsecure`  true | false

Whether to allow insecure connections (client only). The default value is false.

> `rejectUnknownSni` true | false

When the value is true, the server will reject the TLS handshake if the SNI received by the server does not match the domain name of the certificate. The default is false.

> `sni`: server name identification, use for cert verification on client.

Specifies the domain name of the server-side certificate, useful when the connection is established by IP.

> `flow`:  none | xtls-rprx-direct | xtls-rprx-vision 

Flow control mode, used to select the XTLS algorithm. 

NB: xtls-rprx-vision support only tls.



### Reality Settings

```json
{
  "serverName": "www.lovelive-anime.jp",
  "fingerprint": "chrome",
  "shortid": "6ba85179e30d4fc2",
  "flow": "xtls-rprx-vision",
  "spiderx": "",
  "publickey": "MNN-kSKdj_9m3nQKP16wiS3P1tv-ClrQ4B1IBgqySCs"
}
```

> `show`: false,  if true, output debugging information

> `serverName`: One of the server serverNames

> `fingerprint`: "chrome" | "firefox" | "safari" | "randomized"

> `flow`:  xtls-rprx-vision 
 
> `shortid`:  One of the server shortIds

> `publickey`: The public key corresponding to the private key of the server

> `spiderx`: The initial path and parameters of the crawler are recommended to be different for each client



