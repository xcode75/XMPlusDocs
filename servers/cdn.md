## Nginx + CDN [ WS(TLS) / GRPC (TLS) ]

> `Tested with Trojan, Vmess and Vless`  Use below ports in nginx server for cloudflare

#### Enable websocket and grpc traffic on cloudflare

Login to Cloudflare > Network > Enable websocket and grpc


HTTP(NO TLS) ports supported by Cloudflare
```
80
8080
8880
2052
2082
2086
2095
```

HTTPS(TLS) ports supported by Cloudflare
```
443
2053
2083
2087
2096
8443
```

Install Nginx on backend server

CentOS：
```
 yum -y update
 
 yum install -y epel-release
 
 yum install -y nginx
```

Ubuntu/Debian:

```
 apt update
 
 apt install nginx -y
```

### Nginx Config

NB: There are two location added in this cdn.conf example, you only need to add and delete what you need.

Create a custom config file and put in /etc/nginx/conf.d/ directory.

cdn.conf

#### -------------------------------GRPC/Websocket----------------------------------
```
server {
    listen 8080;
    listen [::]:8080;
    server_name  x.tld.com;
    index index.html;
    root /var/www/html;

    set_real_ip_from 103.21.244.0/22;
    set_real_ip_from 103.22.200.0/22;
    set_real_ip_from 103.31.4.0/22;
    set_real_ip_from 104.16.0.0/12;
    set_real_ip_from 108.162.192.0/18;
    set_real_ip_from 131.0.72.0/22;
    set_real_ip_from 141.101.64.0/18;
    set_real_ip_from 162.158.0.0/15;
    set_real_ip_from 172.64.0.0/13;
    set_real_ip_from 173.245.48.0/20;
    set_real_ip_from 188.114.96.0/20;
    set_real_ip_from 190.93.240.0/20;
    set_real_ip_from 197.234.240.0/22;
    set_real_ip_from 198.41.128.0/17;
    set_real_ip_from 2400:cb00::/32;
    set_real_ip_from 2606:4700::/32;
    set_real_ip_from 2803:f800::/32;
    set_real_ip_from 2405:b500::/32;
    set_real_ip_from 2405:8100::/32;
    set_real_ip_from 2c0f:f248::/32;
    set_real_ip_from 2a06:98c0::/29;

    real_ip_header CF-Connecting-IP;	

    location /grpc/Tun { #Your server GRPC ServiceName
        if ($content_type !~ "application/grpc") {
            return 404;
        }
        client_max_body_size 0;
        client_body_timeout 60m;
        send_timeout 60m;
        lingering_close always;

        grpc_read_timeout 3m;
        grpc_send_timeout 2m;
        grpc_set_header Host $host;
        grpc_set_header X-Real-IP $remote_addr;
        grpc_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        grpc_pass grpc://127.0.0.1:Your XMPlus Listening Port;   #grpc_pass grpc://127.0.0.1:5500
    }

    #location / Your server Websocket Path {  #Your server Websocket Path
    #    if ($http_upgrade != "websocket") {
    #        return 404;
    #    }
    #    proxy_redirect off;
    #    proxy_pass http://127.0.0.1:Your XMPlus Listening Port; #proxy_pass http://127.0.0.1:5500
    #    proxy_http_version 1.1;
    #    proxy_set_header Upgrade $http_upgrade;
    #    proxy_set_header Connection "upgrade";
    #    proxy_set_header Host $host;
    #    proxy_set_header X-Real-IP $remote_addr;
    #    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    #} 	
}

```

##### -------------------------------GRPC/Websocket + TLS----------------------------------
```
server {
    listen 443 ssl http2 so_keepalive=on;
    listen [::]:443 ssl http2;
    server_name  x.tld.com;
    index index.html;
    root /var/www/html;
  
	ssl_certificate /your/cert/x.tld.com.crt;
	ssl_certificate_key /your/key/x.tld.com.key;
	ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers           ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
 
    client_header_timeout 52w;
    keepalive_timeout 52w;

    set_real_ip_from 103.21.244.0/22;
    set_real_ip_from 103.22.200.0/22;
    set_real_ip_from 103.31.4.0/22;
    set_real_ip_from 104.16.0.0/12;
    set_real_ip_from 108.162.192.0/18;
    set_real_ip_from 131.0.72.0/22;
    set_real_ip_from 141.101.64.0/18;
    set_real_ip_from 162.158.0.0/15;
    set_real_ip_from 172.64.0.0/13;
    set_real_ip_from 173.245.48.0/20;
    set_real_ip_from 188.114.96.0/20;
    set_real_ip_from 190.93.240.0/20;
    set_real_ip_from 197.234.240.0/22;
    set_real_ip_from 198.41.128.0/17;
    set_real_ip_from 2400:cb00::/32;
    set_real_ip_from 2606:4700::/32;
    set_real_ip_from 2803:f800::/32;
    set_real_ip_from 2405:b500::/32;
    set_real_ip_from 2405:8100::/32;
    set_real_ip_from 2c0f:f248::/32;
    set_real_ip_from 2a06:98c0::/29;

    real_ip_header CF-Connecting-IP;	

    location /grpc/Tun { #Your server GRPC ServiceName
        if ($content_type !~ "application/grpc") {
            return 404;
        }
        client_max_body_size 0;
        client_body_timeout 60m;
        send_timeout 60m;
        lingering_close always;

        grpc_read_timeout 3m;
        grpc_send_timeout 2m;
        grpc_set_header Host $host;
        grpc_set_header X-Real-IP $remote_addr;
        grpc_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        grpc_pass grpc://127.0.0.1:Your XMPlus Listening Port;   #grpc_pass grpc://127.0.0.1:5500
    } 	

    #location / Your server Websocket Path {  #Your server Websocket Path
    #    if ($http_upgrade != "websocket") {
    #        return 404;
    #    }
    #    proxy_redirect off;
    #    proxy_pass http://127.0.0.1:Your XMPlus Listening Port; #proxy_pass http://127.0.0.1:5500
    #    proxy_http_version 1.1;
    #    proxy_set_header Upgrade $http_upgrade;
    #    proxy_set_header Connection "upgrade";
    #    proxy_set_header Host $host;
    #    proxy_set_header X-Real-IP $remote_addr;
    #    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    #}
}
```

##### Enable and Restart Nginx

```
systemctl enable nginx
systemctl start nginx
systemctl restart nginx

```

1. Set your XMPlus server port to the nginx listen port. Example 443 for this setup

2. Set your XMPlus Listening IP to 127.0.0.1

3. Set your XMPlus Listening Port cannot be same as Nginx port(server port)

4. Set your XMPlus Cert Mode to None

##### Restart XMPlus backend

> `xmplus restart`

##### Enable Cloudflare(proxied) CDN

Turn on from cloudflare proxy for cdn.


Arvancloud CDN (Only support WS) IP Range

````
    set_real_ip_from 185.143.232.0/22;
    set_real_ip_from 92.114.16.80/28;
    set_real_ip_from 2.146.0.0/28;
    set_real_ip_from 46.224.2.32/29;
    set_real_ip_from 83.123.255.56/31;
    set_real_ip_from 188.229.116.16/29;
    set_real_ip_from 164.138.128.28/31;
    set_real_ip_from 94.182.182.28/30;
    set_real_ip_from 185.17.115.176/30;
    set_real_ip_from 5.213.255.36/31;
    set_real_ip_from 185.228.238.0/28;
    set_real_ip_from 94.182.153.24/29;
    set_real_ip_from 94.101.182.0/27;
    set_real_ip_from 158.255.77.238/31;
    set_real_ip_from 81.12.28.16/29;
    set_real_ip_from 176.65.192.202/31;
    set_real_ip_from 2.144.3.128/28;
    set_real_ip_from 89.45.48.64/28;
    set_real_ip_from 37.32.16.0/27;
    set_real_ip_from 37.32.17.0/27;
    set_real_ip_from 37.32.18.0/27;
    set_real_ip_from 37.32.19.0/27;
    set_real_ip_from 185.215.232.0/22;

    real_ip_header ar-real-ip;
````
