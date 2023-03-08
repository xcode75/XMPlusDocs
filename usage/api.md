# Client

##### Client API

| URL            | Method  | Description                    |
|----------------|---------|-----------------------------------|
| /token         | POST    | [Get Token](#1Get Token)    |
| /servers       | POST    | [login Account](#2Get Servers)       |
| /stats         | POST    | [Account Statistics](#3Get Stats)  |


### 1.Get Token

- POST `{{apihost}}/api/v2/client/token`

- request headers

	> XMPus-API-Token: `md5(<your_api_key>)`
	> Content-Type:    `application/json`

- request parameters (json body)

```json
 {
  "email": "xmplus@xmplus.dev",
  "passwd": "Ax345@78",
}
```

| Parameter name  |  Type  | Required  | Description   |
|----------|--------|-----|------|
| email    | string | ✔︎  | Email |
| passwd   | string | ✔︎  | Login Password   |

- successful return example `json`

```json
{
    "ret": 1,
    "status": "success",
    "data": {
        "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL3d3dy54bXBsdXMuZGV2IiwiYXVkIjoiaHR0cGM6Ly93d3cueG1wbHVzLmRldiIsImlhdCI6MTY3NzU0MTE4NSwiZXhwIjoxNjc3NzEzOTg1LCJlbWFpbCI6IndlYm1hc3RlckB4bXBsdXMuZGV2In0.1e4Vwk1XgVtmDHoOIVRFmcJPlE7t9Q27-opiWWhXdFE"
    }
}
```

### 2.Get Servers

- POST `{{apihost}}/api/v2/client/servers`

- request headers

	> XMPus-API-Token: `md5(<your_api_key>)`
	> Content-Type:    `application/json`

- request parameters (json body)

```json
{
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL3d3dy54bXBsdXMuZGV2IiwiYXVkIjoiaGR0cHM6Ly93d3cueG1wbHVzLmRldiIsImlhdCI6MTY3NzU0MDIyNCwiZXhwIjoxNjc3NzEzMDI0LCJlbWFpbCI6IndlYm1hc3RlckB4bXBsdXMuZGV2In0.mzxFglyYi7euqfRRewEQEBLqIH7OpF1HdWLRNUabHV0"
}
```

| Parameter name  |  Type  | Required  | Description   |
|----------|--------|-----|------|
| token           | string    |    ✔︎   | Authentication token |

- successful return example `json`

```json
{
    "ret": 1,
    "status": "success",
    "servers": [
        {
            "type": "vmess",
            "remark": "🇬🇧 UK | X1.5 | AMZ",
            "address": "ukser.gbxcloud.com",
            "port": 443,
            "network": "tcp",
            "password": "9d6023b1-0948-4827-b9e9-9849b0fb5062",
            "alterid": 0,
            "encryption": "auto",
            "sni": "ukser.gbxcloud.com",
            "security": "",
            "allowinsecure": true,
            "header": "none",
            "fingerprint": "chrome",
            "uri": "vmess://eyJ2IjoiMiIsInBzIjoiXHVkODNjXHVkZGVjXHVkODNjXHVkZGU3IFVLIHwgWDEuNSB8IEFNGiIsImFkZCI6InVrc2VyLmdieGNsb3VkLmNvbSIsInBvcnQiOiI0NDMiLCJpZCI6IjlkNjAyM2IxLTA5NDgtNDgyNy1iOWU5LTk4NDliMGZiNTA2MiIsImFpZCI6IjAiLCJuZXQiOiJ0Y3AiLCJ0eXBlIjoibm9uZSIsImhvc3QiOiIiLCJwYXRoIjoiIn0="
        }
    ]
}
```

### 3. Get Stats

- POST `{{apihost}}/api/v2/client/stats`

- request headers

	> XMPus-API-Token: `md5(<your_api_key>)`
	> Content-Type:    `application/json`

- request parameters (json body)

```json
{
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL3d3dy54bXBsdXMuZGV2IiwiYXVkIjoiaGR0cHM6Ly93d3cueG1wbHVzLmRldiIsImlhdCI6MTY3NzU0MDIyNCwiZXhwIjoxNjc3NzEzMDI0LCJlbWFpbCI6IndlYm1hc3RlckB4bXBsdXMuZGV2In0.mzxFglyYi7euqfRRewEQEBLqIH7OpF1HdWLRNUabHV0"
}
```

| Parameter name  |  Type  | Required  | Description   |
|----------|--------|-----|------|
| token           | string    |    ✔︎   | Authentication token |

- successful return example `json`

```json
{
    "ret": 1,
    "status": "success",
    "info": {
        "username": "admin",
        "email": "webmaster@xmplus.dev",
        "money": "USD 0.00",
        "commission": "USD 0.00",
        "iplimit": 2,
        "onlineip": "0/2",
        "speedlimit": "1 Gb/s",
        "expire_at": "2220-04-15 08:18:51",
        "group": "VIP1"
    },
    "data": {
        "package": null,
        "used": "0 B",
        "remaining": "100 G",
        "total": "100 G"
    },
    "sublink": "https://www.xmplus.dev/link/Gb0sxRcR5LfihSpX1Bvx?config=1"
}
```