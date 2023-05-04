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
| Server Transit       | √     |  √    |   √    |  √   |    X     |


### one-click installation

```
bash <(curl -Ls https://raw.githubusercontent.com/xcode75/XMPlus/install/install.sh)
```

### Backend configuration

vi /etc/XMPlus/config.yml  or can use any editor

```
Log:
  Level: warning # Log level: none, error, warning, info, debug 
  AccessPath: # /etc/XMPlus/access.Log
  ErrorPath: # /etc/XMPlus/error.log
DnsConfigPath:  #/etc/XMPlus/dns.json
RouteConfigPath: # /etc/XMPlus/route.json
InboundConfigPath: # /etc/XMPlus/inbound.json
OutboundConfigPath: # /etc/XMPlus/outbound.json
ConnectionConfig:
  Handshake: 8 
  ConnIdle: 300 
  UplinkOnly: 0 
  DownlinkOnly: 0 
  BufferSize: 64
Nodes:
  -
    ApiConfig:
      ApiHost: "https://www.tld.om"
      ApiKey: "123"
      NodeID: 1
      Timeout: 30 
    ControllerConfig:
      CertConfig:
        Email: author@xmplus.dev                    # Required when Cert Mode is not none
        CertFile: /etc/XMPlus/node1.xmplus.dev.crt  # Required when Cert Mode is file
        KeyFile: /etc/XMPlus/node1.xmplus.dev.key   # Required when Cert Mode is file
        Provider: cloudflare                        # Required when Cert Mode is dns
        CertEnv:                                    # Required when Cert Mode is dns
          CLOUDFLARE_EMAIL:                         # Required when Cert Mode is dns
          CLOUDFLARE_API_KEY:                       # Required when Cert Mode is dns
      RealityConfigs:
        Show: false   # Show REALITY debug
        Dest: www.lovelive-anime.jp:443   # Required, Same as fallback
        Xver : 0   # Send PROXY protocol version, 0 for disable
        ServerNames:    # Required, list of available serverNames for the client, * wildcard is not supported at the moment.
          - www.lovelive-anime.jp
        PrivateKey: MH0gmvlQQJjgM0pE2XhPMpcuEVtm2pTc0UKCY1oEvFU   # Required, execute './xray x25519' to generate.
        MinClientVer:   # Optional, minimum version of Xray client, format is x.y.z.
        MaxClientVer:   # Optional, maximum version of Xray client, format is x.y.z.
        MaxTimeDiff: 0  # Optional, maximum allowed time difference, unit is in milliseconds.
        ShortIds:    # Required, list of available shortIds for the client, can be used to differentiate between different clients.
          - 6ba85179e30d4fc2
```

> `ApiHost` :  your website address. eg, https://www.tld.com  and not this https://www.tld.com/

> `ApiKey`: your api key, an be found in admin settings > API settings > API Key

> `NodeID`:  The server id number after creating a server in the admin panel

> `Timeout`: time before no response from api.

> `CertConfig`: you only need to set these if you set cert mode to DNS when creating server. it is used to resolve cert address when generating certificate when tls is enable.

> `RealityConfigs`  

### Importan Notice

Do not install backend on same server as frontend if you intend to enable tls on the backend.

Reason: certificate generation need port 80 to be free, but since the frontend is running on port 80/443, backend will panic, and cannot start.


### allow xray(eg, 443) port through firewall


DEBIAM/UBUNTU:
```
apt install firewalld
```

CENTOS:
```
yum install firewalld
```

```
systemctl enable firewalld
systemctl start firewalld


firewall-cmd --permanent --add-port=1-65500/tcp
firewall-cmd --permanent --add-port=1-65500/udp
firewall-cmd --reload
```

### Backend Transit(Relay) Client -> Node A -> Node B -> Destination(www.google.com)

NOTE:

 > To use transit both servers must be working normally and be able to connect directly.
 
 > Node B cannot use nginx reverse proxy and lsitening ip 127.0.0.1


Node A and B must have the XMPlus backend installed

Node A -> Transit Type -> XMPlus backend

Node A -> Transit Server -> select target Node B