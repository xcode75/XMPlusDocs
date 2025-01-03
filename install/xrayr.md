# XrayR backend installation

A Xray backend framework that can easily support many panels.

[XrayR-doc](https://xrayr-project.github.io/XrayR-doc/)

[XrayR-Github](https://github.com/XrayR-project/XrayR)

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
| Server Transit       | x     |  x    |   x    |  √   |    X     |


### one-click installation

```
wget -N https://raw.githubusercontent.com/wyx2685/XrayR-release/master/install.sh && bash install.sh
```

### Backend configuration

vi /etc/XrayR/config.yml  or can use any editor to chnage default config.yml and use below example

```
Log:
  Level: warning # Log level: none, error, warning, info, debug 
  AccessPath: # /etc/XrayR/access.Log
  ErrorPath: # /etc/XrayR/error.log
DnsConfigPath: # /etc/XrayR/dns.json # Path to dns config, check https://xtls.github.io/config/dns.html for help
RouteConfigPath: # /etc/XrayR/route.json # Path to route config, check https://xtls.github.io/config/routing.html for help
InboundConfigPath: # /etc/XrayR/custom_inbound.json # Path to custom inbound config, check https://xtls.github.io/config/inbound.html for help
OutboundConfigPath: # /etc/XrayR/custom_outbound.json # Path to custom outbound config, check https://xtls.github.io/config/outbound.html for help
ConnectionConfig:
  Handshake: 4 # Handshake time limit, Second
  ConnIdle: 30 # Connection idle time limit, Second
  UplinkOnly: 2 # Time limit when the connection downstream is closed, Second
  DownlinkOnly: 4 # Time limit when the connection is closed after the uplink is closed, Second
  BufferSize: 64 # The internal cache size of each connection, kB
Nodes:
  - PanelType: "SSpanel" # Panel type: SSpanel, NewV2board, PMpanel, Proxypanel, V2RaySocks, GoV2Panel
    ApiConfig:
      ApiHost: "https://www.xmplus.dev"
      ApiKey: "xxxgjyuyiy"
      NodeID: 1
      NodeType: V2ray # Node type: V2ray, Shadowsocks, Trojan, Shadowsocks-Plugin
      Timeout: 30 # Timeout for the api request
      EnableVless: true # Enable Vless for V2ray Type
      VlessFlow: "xtls-rprx-vision" # Only support vless
      SpeedLimit: 0 # Mbps, Local settings will replace remote settings, 0 means disable
      DeviceLimit: 0 # Local settings will replace remote settings, 0 means disable
      RuleListPath: # /etc/XrayR/rulelist Path to local rulelist file
      DisableCustomConfig: false # disable custom config for sspanel
    ControllerConfig:
      ListenIP: 0.0.0.0 # IP address you want to listen
      SendIP: 0.0.0.0 # IP address you want to send pacakage
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
      EnableDNS: false # Use custom DNS config, Please ensure that you set the dns.json well
      DNSType: AsIs # AsIs, UseIP, UseIPv4, UseIPv6, DNS strategy
      EnableProxyProtocol: false # Only works for WebSocket and TCP
      AutoSpeedLimitConfig:
        Limit: 0 # Warned speed. Set to 0 to disable AutoSpeedLimit (mbps)
        WarnTimes: 0 # After (WarnTimes) consecutive warnings, the user will be limited. Set to 0 to punish overspeed user immediately.
        LimitSpeed: 0 # The speedlimit of a limited user (unit: mbps)
        LimitDuration: 0 # How many minutes will the limiting last (unit: minute)
      GlobalDeviceLimitConfig:
        Enable: false # Enable the global device limit of a user
        RedisAddr: 127.0.0.1:6379 # The redis server address
        RedisPassword: YOUR PASSWORD # Redis password
        RedisDB: 0 # Redis DB
        Timeout: 5 # Timeout for redis request
        Expiry: 60 # Expiry time (second)
      EnableFallback: false # Only support for Trojan and Vless
      FallBackConfigs:  # Support multiple fallbacks
        - SNI: # TLS SNI(Server Name Indication), Empty for any
          Alpn: # Alpn, Empty for any
          Path: # HTTP PATH, Empty for any
          Dest: 80 # Required, Destination of fallback, check https://xtls.github.io/config/features/fallback.html for details.
          ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for disable
      DisableLocalREALITYConfig: true  # disable local reality config
      EnableREALITY: false # Enable REALITY
      REALITYConfigs:
        Show: false # Show REALITY debug
        Dest: www.smzdm.com:443 # Required, Same as fallback
        ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for disable
        ServerNames: # Required, list of available serverNames for the client, * wildcard is not supported at the moment.
          - www.smzdm.com
        PrivateKey: yBaw532IIUNuQWDTncozoBaLJmcd1JZzvsHUgVPxMk8 # Required, execute './xray x25519' to generate.
        MinClientVer: # Optional, minimum version of Xray client, format is x.y.z.
        MaxClientVer: # Optional, maximum version of Xray client, format is x.y.z.
        MaxTimeDiff: 0 # Optional, maximum allowed time difference, unit is in milliseconds.
        ShortIds: # Required, list of available shortIds for the client, can be used to differentiate between different clients.
          - "6ba85179e30d4fc2"
          - 0123456789abcdef
      CertConfig:
        CertMode: none # Option about how to get certificate: none, file, http, tls, dns. Choose "none" will forcedly disable the tls config.
        CertDomain: "node1.test.com" # Domain to cert
        CertFile: /etc/XrayR/cert/node1.test.com.cert # Provided if the CertMode is file
        KeyFile: /etc/XrayR/cert/node1.test.com.key
        Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
        Email: test@me.com
        DNSEnv: # DNS ENV option used by DNS provider
          ALICLOUD_ACCESS_KEY: aaa
          ALICLOUD_SECRET_KEY: bbb
```

> `ApiHost` :  your website address. eg, https://www.tld.com  and not this https://www.tld.com/

> `ApiKey`: your api key, an be found in admin settings > API settings > API Key

> `NodeID`:  The server id number after creating a server in the admin panel

> `NodeType`: If frontend server settings is vmess or vless select V2ray

> `EnableVless`: If your frontend server settings is vless, set to true

> `EnableXTLS`: If your frontend server settings is XTLS, set to true

> `Timeout`: time before no response from api.

> `UpdatePeriodic`:  time(seconds) after backend send data to the frontend and retrieve new data from frontend. Least time is 60 seconds, if you change theis value, remember to also change in admin panel > api settings > Update Period(s)

> `ListenIP`: If you are using nginx set listening ip to 127.0.0.1 and CertMode to none in config.yml

> `CertConfig`: you need to set these if you set CertMode is not none

>  `DisableLocalREALITYConfig`: true # use reality settings from xmplus panel

Do not make any chnages to these options below in the config

> AutoSpeedLimitConfig

> GlobalDeviceLimitConfig

> `SpeedLimit`: 0 # Mbps, Local settings will replace remote settings, 0 means disable

> `DeviceLimit`: 0 # Local settings will replace remote settings, 0 means disable

> RuleListPath``: # /etc/XrayR/rulelist Path to local rulelist file
	  
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

>  Note: XrayR does not support frontend  Transit(Relay)

## XrayR GlobalDeviceLimitConfig

### Install redis-server on a dedicated server

# Server

sudo apt update -y
sudo apt upgrade -y
sudo apt install redis-server

Edit ```/etc/redis/redis.conf``` and set ```bind 0.0.0.0``` and ```requirepass yourownpass```

Edit ```/etc/systemd/system/redis.service```  add Type=notify under service

```
[Service]
Type=notify

ExecStart=/usr/bin/redis-server /etc/redis.conf --supervised systemd
```

Restart redis Server

```sudo systemctl daemon-reload```
```sudo systemctl enable redis-server.service```
```sudo systemctl restart redis```
```sudo systemctl status redis```

Check if redis is running and install redis client on all backend servers

### Install redis-client on XrayR backend servers

# Client

sudo apt-get install redis-tools
sudo apt install redis-tools
	

## Fill in details for XrayR GlobalDeviceLimitConfig

```
      GlobalDeviceLimitConfig:
        Enable: false # Enable the global device limit of a user
        RedisNetwork: tcp # Redis protocol, tcp or unix
        RedisAddr: 127.0.0.1:6379 # Redis server address, or unix socket path
        RedisUsername: # Redis username
        RedisPassword: YOUR PASSWORD # Redis password
        RedisDB: 0 # Redis DB
        Timeout: 5 # Timeout for redis request
        Expiry: 60 # Expiry time (second)
```	
