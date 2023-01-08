# Security Settings

NB: Server port and listening port must be the same, unless you are using other method such as nginx(fill nginx port for server port) to process tls.

### TLS/XTLS Settings (XTLS currently only supports three transport(Network) methods: TCP, KCP, and DomainSocket.) 


### TLS Settings

```json
{
  "serverName": "xmplus.dev",
  "rejectUnknownSni": true,
  "allowInsecure": false,
  "fingerprint": "chrome"
}
```

### XTLS Settings

```json
{
  "serverName": "xmplus.dev",
  "rejectUnknownSni": true,
  "allowInsecure": false,
  "fingerprint": "chrome",
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

> `flow`:  none | xtls-rprx-direct | xtls-rprx-origin | xtls-rprx-origin-udp443 | xtls-rprx-vision | xtls-rprx-vision,none

Flow control mode, used to select the XTLS algorithm. 

NB: xtls-rprx-vision support only tls.


Check documentation here [TLS Object](https://xtls.github.io/Xray-docs-next/config/transport.html#tlsobject)