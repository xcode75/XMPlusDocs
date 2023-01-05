# XMPlus backend installation

The XMPlus backend is a customized version of XRayR (one server supports multiple nodes)

## Features

|Function             | vmess | vless | trojan | ss   | ss-plugin|
| ----------------| ----- | ----- | -------| -----|----------|
| Get Server Info     | √     |  √    |   √    |  √   |    √     |
| Get User Info     | √     |  √    |   √    |  √   |    √     |
| Report User Stats     | √     |  √    |   √    |  √   |    √     |
| Report Server Stats   | √     |  √    |   √    |  √   |    √     |
| Auto Apply tls Cert  | √     |  √    |   √    |  √   |    √     |
| Auto-Renew tls Cert  | √     |  √    |   √    |  √   |    √     |
| Online Statistics     | √     |  √    |   √    |  √   |    √     |
| Online User IPlinit   | √     |  √    |   √    |  √   |    √     |
| Detection Rules        | √     |  √    |   √    |  √   |    √     |
| Server Speedlimit     | √     |  √    |   √    |  √   |    √     |
| User Speedlimit     | √     |  √    |   √    |  √   |    √     |
| Customize DNS       | √     |  √    |   √    |  √   |    √     |
| Server Relay       | √     |  √    |   √    |  √   |    X     |


### one-click installation

```
bash <(curl -Ls https://raw.githubusercontent.com/xcode75/XMPlus/install/install.sh)
```

### Backend configuration

vi /etc/XMPlus/config.yml

```
Log:
  Level: info # Log level: none, error, warning, info, debug 
  AccessPath: # /etc/XMPlus/access.Log
  ErrorPath: # /etc/XMPlus/error.log
DnsConfigPath:  /etc/XMPlus/dns.json
RouteConfigPath: # /etc/XMPlus/route.json
InboundConfigPath: # /etc/XMPlus/inbound.json
OutboundConfigPath: # /etc/XMPlus/outbound.json
ConnectionConfig:
  Handshake: 8 
  ConnIdle: 150 
  UplinkOnly: 0 
  DownlinkOnly: 0 
  BufferSize: 64
Nodes:
  -
    ApiConfig:
      ApiHost: "http://127.0.0.1"
      ApiKey: "123"
      NodeID: 1
      Timeout: 30 
    ControllerConfig:
      UpdatePeriodic: 60
      CertConfig:
        Provider: cloudflare
        Email: author@xmplus.dev
        CertEnv:
          CLOUDFLARE_EMAIL: 
          CLOUDFLARE_API_KEY: 
          
#  -
#    ApiConfig:
#      ApiHost: "http://127.0.0.1"
#      ApiKey: "123"
#      NodeID: 2
#      Timeout: 30 
#    ControllerConfig:
#      UpdatePeriodic: 60
#      CertConfig:
#        Provider: cloudflare
#        Email: author@xmplus.dev
#        CertEnv:
#          CLOUDFLARE_EMAIL: 
#          CLOUDFLARE_API_KEY: 
```

> `ApiHost` :  your website address. eg, https://www.tld.com  and not this https://www.tld.com/

> `ApiKey`: your api key, an be found in admin settings > API settings > API Key

> `NodeID`:  The server id umber after creating a server in the admin panel

> `Timeout`: time before no response from api.

> `UpdatePeriodic`:  time(seconds) after backend send data to the fromt end and retrieve nre data from frontend . least is 60 seconds, if you change theis value, remember to also change in admin panel > api settings > Update Period(s)

> `CertConfig`: you only need to set these if you set cert mode to DNS when creating server. it is used to resolve cert address when generating certificate when tls is enable.

### Importan Notice
Do not install backend on same server as frontend if you intend to enable tls on the backend.

Reason: certificate generation need port 80 to be free, but since frontend is running on port 80, backend will panic, and cannot start.


### Backend Transit(Tunnel) (Node A -> Transit -> Node B)

The source and target nodes must have the XMPlus backend installed

Node A -> transit mode -> XMPlus backend

Node A -> transit node -> select target node B