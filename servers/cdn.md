## Nginx + Cloudflare CDN [ WS(TLS) / GRPC (TLS) ]

> `Tested with Trojan, Vmess and Vless`  Use below ports in nginx server

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

Ports supported by Cloudflare, but with caching disabled
```
2052
2053
2082
2083
2086
2087
2095
2096
8880
8443
```
Install Nginx on backend server

CentOS：
```
 yum update
 
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

##### -------------------------------GRPC + TLS----------------------------------
```
server {
	listen 443 ssl http2  so_keepalive=on;
	listen [::]:443 ssl http2; 
	server_name x.tld.com;

	index index.html;
	root /var/www/html;

	ssl_certificate /your/cert/x.tld.com.crt;
	ssl_certificate_key /your/key/x.tld.com.key;
	ssl_protocols TLSv1.2 TLSv1.3;
	ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
	
	client_header_timeout 52w;
    keepalive_timeout 52w;
	
	location /Your server GRPC ServiceName { #Your server GRPC ServiceName
		if ($content_type !~ "application/grpc") {
			return 404;
		}
		client_max_body_size 0;
		client_body_buffer_size 512k;
		grpc_set_header X-Real-IP $remote_addr;
		grpc_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		client_body_timeout 52w;
		grpc_read_timeout 52w;
		grpc_pass grpc://127.0.0.1:Your server settings Listening Port; #grpc_pass grpc://127.0.0.1:5500
	}	
}

```

##### -------------------------------Websocket + TLS------------------------------
```
server {
	listen 443 ssl http2  so_keepalive=on;
	listen [::]:443 ssl http2; 
	server_name x.tld.com;

	index index.html;
	root /var/www/html;

	ssl_certificate /your/cert/x.tld.com.crt;
	ssl_certificate_key /your/key/x.tld.com.key;
	ssl_protocols TLSv1.2 TLSv1.3;
	ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
	
	client_header_timeout 52w;
    keepalive_timeout 52w;

	location /Your server Websocket Path {  #Your server Websocket Path
      	if ($http_upgrade != "websocket") {
          	return 404;
        }
      	proxy_redirect off;
      	proxy_pass http://127.0.0.1:Your XMPlus Listening Port; #proxy_pass http://127.0.0.1:5500
      	proxy_http_version 1.1;
      	proxy_set_header Upgrade $http_upgrade;
      	proxy_set_header Connection "upgrade";
      	proxy_set_header Host $host;
      	proxy_set_header X-Real-IP $remote_addr;
      	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }	
}
```


##### Enable and Restart Nginx

```
systemctl enable firewalld
systemctl start firewalld
systemctl restart nginx
```

1. Set your XMPlus server port to the nginx listen port. Example 443 for this setup

2. Set your XMPlus Listening IP to 127.0.0.1

##### Restart XMPlus backend

> `xmplus restart`

##### Enable Cloudflare(proxied) CDN

Option 1. Turn on from server settings if you have already added zone.

Option 2. Turn on from cloudflare dns tab for the host.
