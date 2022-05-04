---
title: Setup a site from scratch
date: 2021-09-10 23:05:00
updated: 2021-12-27 15:22:00
categories:
- Tech
tags:
- Web
- Server
---

A record of mine setup of web hosting, domain etc.

<!-- more -->

# Domain

## Purchase

[GoDaddy](https://hk.godaddy.com/) supports Alipay.

## DNS

Modify the `A` record:
- name: `*`
- value: `[IP ADDRESS]`

## SSL

I've followed this [tutorial](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-20-04) to install Let's Encrypt certificate but it didn't turn out well. It seems that install wildcard certificate using "certbot" requires some cumbersome steps. Nevertheless, this tutorial offers great instruction on how to configure your Ubuntu vps properly after deployment.

[FreeSSL.cn](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-20-04) is somewhat straightforward to use. The drawback is that you have to renew your Let's Encrypt certificate every 3 months.
![enter image description here](/attach/bcq26.png)

# Fire wall

For Ubuntu 20.04
```bash
ufw allow ssh
ufw allow http
ufw allow https
ufw enable
```

# Apache2

## Installation
```bash
# Debian
sudo apt install apache2
# CentOS
sudo yum install httpd
```

## Enable SSL

For Ubuntu:
```bash
a2enmod ssl
```

## Disable directory listing
On Ubuntu, edit /etc/apache2/apache2.conf. In
```
<Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
```
remove `Indexes`.

## HTTP to HTTPS
Enable module rewrite on Ubuntu:
```sh
a2enmod rewrite
```
conf file
```
<VirtualHost *:80>
	RewriteEngine On
	RewriteCond %{HTTPS} off
	RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI}
</VirtualHost>
```

## CDN configure
Enable module headers, rewrite on Ubuntu
```
a2enmod headers rewrite
```
conf file
```
RewriteEngine On
RewriteCond %{QUERY_STRING} (^|&)name=(.*?)(&|$)
RewriteRule ^(.*)$ $1 [E=NAME:%2]

Header set Content-Disposition "attachment"
Header set Content-Disposition "attachment; filename=\"%{NAME}e\"" env=NAME
Header set Access-Control-Allow-Origin "*"
```
In your html file, a download link should be
```html
<a href="https://cdn.example.com/fileid?name=info.txt">click</a>
```

## Proxy
Enable module proxy-http on Ubuntu:
```
a2enmod proxy-http
```
conf file:
```
<Location />
	ProxyPass http://127.0.0.1:8000/
	ProxyPassReverse http://127.0.0.1:8000/
</Location>
```

## [Code server](https://github.com/cdr/code-server)
Enable module proxy-http, proxy-wstunnel, rewrite on Ubuntu:
```
a2enmod proxy-http proxy-wstunnel rewrite
```
conf file:
```
RewriteEngine On
RewriteCond %{HTTP:Upgrade} =websocket [NC]
RewriteRule /(.*) ws://0.0.0.0:8080/$1 [P,L]
RewriteCond %{HTTP:Upgrade} !=websocket [NC]
RewriteRule /(.*) http://0.0.0.0:8080/$1 [P,L]

<Location />
	ProxyPass http://0.0.0.0:8080
	ProxyPassReverse http://0.0.0.0:8080
</Location>
```

## Jupyter notebook

Enable module proxy-http, proxy-wstunnel, headers on Ubuntu:
```
a2enmod proxy-http proxy-wstunnel headers
```
conf file:
```
RequestHeader set Origin "http://127.0.0.1:8888"

<Location />
	ProxyPass http://127.0.0.1:8888/
	ProxyPassReverse http://127.0.0.1:8888/
</Location>

<Location /api/kernels>
	ProxyPass ws://127.0.0.1:8888/api/kernels
	ProxyPassReverse ws://127.0.0.1:8888/api/kernels
</Location>
```

# Nginx

Nginx is faster and easier to config, compared to Apache.

## Install

```sh
sudo apt install nginx
```

The configuration folder: `/etc/nginx/`

## Config ssl and http/https

Create `ssl.conf` and `https.conf` in `/etc/nginx/snippets`

In `ssl.conf`:
```
ssl_session_timeout 1d;
ssl_session_cache shared:SSL:50m;
ssl_session_tickets off;

ssl_protocols TLSv1.2;
ssl_ciphers ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES256-SHA384;
ssl_ecdh_curve secp384r1;
ssl_prefer_server_ciphers on;

ssl_stapling on;
ssl_stapling_verify on;

add_header Strict-Transport-Security "max-age=15768000; includeSubdomains; preload";
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;

ssl_certificate /path/to/fullchain.pem;
ssl_certificate_key /path/to/privkey.pem;
ssl_trusted_certificate /path/to/fullchain.pem;
```

In `https.conf`:
```
listen 80;
listen 443 ssl;

if ($scheme != "https") {
    return 301 https://$server_name$request_uri;
}
```

## The default/fallback site

Modify `/etc/nginx/sites-available/default`

```
server {
    include snippets/ssl.conf;   

    listen 80 default_server;
    listen 443 ssl default_server;

    server_name _;

    return 404;
}

server {
    include snippets/ssl.conf;   
    include snippets/https.conf;   

    server_name example.com www.example.com;

    add_header Content-Type "text/plain; charset=utf-8";
    return 200 "Hi there";
}
```

## A static site

Create `/etc/nginx/sites-available/test`

```
server {
    include snippets/ssl.conf;
    include snippets/https.conf;

    server_name test.example.com;
    root /var/www/test;
}
```

Enable it:
```
ln -s /etc/nginx/sites-available/test /etc/nginx/sites-enabled
```

Refresh Nginx:
```
sudo service nginx reload
```

## Reverse proxy

```
server {
    include snippets/ssl.conf;
    include snippets/https.conf;

    server_name test.example.com;
    root /var/www/test;

    location /api/ {
        proxy_pass http://127.0.0.1:8000/test/;
    }
}
```

## CDN

```
server {
    include snippets/ssl.conf;
    include snippets/https.conf;

    server_name cdn.example.com;
    root /var/www/cdn;

    location / {
        add_header Access-Control-Allow-Origin *;
        add_header Content-Disposition attachment;
        if ($arg_name) {
            add_header Content-Disposition "attachment; filename=$arg_name";
        }
    }
}
```