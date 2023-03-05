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

> `NodeID`:  The server id number after creating a server in the admin panel

> `Timeout`: time before no response from api.

> `UpdatePeriodic`:  time(seconds) after backend send data to the frontend and retrieve new data from frontend. Least time is 60 seconds, if you change theis value, remember to also change in admin panel > api settings > Update Period(s)

> `CertConfig`: you only need to set these if you set cert mode to DNS when creating server. it is used to resolve cert address when generating certificate when tls is enable.


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