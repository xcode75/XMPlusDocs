# Security Settings

NB: Server port and listening port must be the same, unless you are using other method such as nginx(fill nginx port for server port) to process tls.

### TLS Settings 

```json
{
  "serverName": "xmplus.dev",
  "rejectUnknownSni": true,
  "allowInsecure": false,
  "fingerprint": "chrome",
  "sni": "xmplus.dev"
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


### Reality Settings (Frontend / Client)

[Check official readme here] (https://github.com/XTLS/REALITY#readme)

[Generate Private and Public Keys Here] (https://go.dev/play/p/N5kQhIjtye7)

```json
{
  "show" : false,
  "dest": "www.lovelive-anime.jp:443",
  "privatekey" : "yBaw532IIUNuQWDTncozoBaLJmcd1JZzvsHUgVPxMk8",
  "minclientver":"",
  "maxclientver":"",
  "maxtimediff":0,
  "proxyprotocol":0,
  "shortids" : [
    "6ba85179e30d4fc2"
  ],
  "serverNames": [
    "www.lovelive-anime.jp",
    "www.cloudflare.com"
  ],
  "fingerprint": "chrome",
  "spiderx": "",
  "publickey": "7xhH4b_VkliBxGulljcyPOH-bYUA2dl-XAdZAsfhk04"
}
```


