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
bash <(curl -Ls https://raw.githubusercontent.com/XMPlusDev/XMPlusv1/install/install.sh)
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
      ApiHost: "https://www.xyz.com"
      ApiKey: "123"
      NodeID: 1
      Timeout: 30 
      RuleListPath: # /etc/XMPlus/rulelist Path to local rulelist file
    ControllerConfig:
      EnableDNS: false # Use custom DNS config, Please ensure that you set the dns.json well
      DNSStrategy: AsIs # AsIs, UseIP, UseIPv4, UseIPv6
      CertConfig:
        Email: author@xmplus.dev                    # Required when Cert Mode is not none
        CertFile: /etc/XMPlus/node1.xmplus.dev.crt  # Required when Cert Mode is file
        KeyFile: /etc/XMPlus/node1.xmplus.dev.key   # Required when Cert Mode is file
        Provider: cloudflare                        # Required when Cert Mode is dns
        CertEnv:                                    # Required when Cert Mode is dns
          CLOUDFLARE_EMAIL:                         # Required when Cert Mode is dns
          CLOUDFLARE_API_KEY:                       # Required when Cert Mode is dns
      EnableFallback: false # Only support for Trojan and Vless
      FallBackConfigs:  # Support multiple fallbacks
        - SNI: # TLS SNI(Server Name Indication), Empty for any
          Alpn: # Alpn, Empty for any
          Path: # HTTP PATH, Empty for any
          Dest: 80 # Required, Destination of fallback, check https://xtls.github.io/config/features/fallback.html for details.
          ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for disable
      IPLimit:
        Enable: false # Enable the global ip limit of a user 
        RedisNetwork: tcp # Redis protocol, tcp or unix
        RedisAddr: 127.0.0.1:6379 # Redis server address, or unix socket path
        RedisUsername: default # Redis username
        RedisPassword: YOURPASSWORD # Redis password
        RedisDB: 0 # Redis DB
        Timeout: 5 # Timeout for redis request
        Expiry: 60 # Expiry time (second) 
```

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