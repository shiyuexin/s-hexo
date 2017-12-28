---
title: nginx使用及常用配置
date: 2017-12-28
comments: true
categories: nginx服务
description: nginx使用及常用配置
---

### 关于nginx

Nginx (engine x) 是一个高性能的HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP服务器。
Nginx是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，并在一个BSD-like 协议下发行。
其特点是占有内存少，并发能力强，事实上nginx的并发能力确实在同类型的网页服务器中表现较好。

<!-- more -->

### Nginx安装

#### 1、从官网（http://www.nginx.org）下载最新的Nginx并解压，进入解压目录进行相关安装操作即可，具体如下：

```
$ tar –xvf  nginx-1.8.1.tar
$ cd  nginx-1.8.1
$ sudo  ./configure
$ sudo  make
$sudo  make install
```

#### 2、安装之后，使用nginx –v验证下是否安装完成：

```
$ nginx -V
```
![版本查看](/images/nginx-1.png)

#### 3、开启nginx服务，并打开浏览器地址：127.0.0.1

```
$ sudo  ./nginx  // 开启服务
```

### Nginx配置

```
#user  nobody;
worker_processes  auto; #根据设备cpu的个数 自动选择
 
#error_log  /nginx/nginx-1.8.1/logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
 
#pid        logs/nginx.pid;
 
events {
    worker_connections  1024; ＃允许请求的连接数
}
 
 
http {
    include       mime.types;
    default_type  application/octet-stream;
 
    #log_format main  '$remote_addr - $remote_user[$time_local] "$request" '
    #                  '$status $body_bytes_sent"$http_referer" '
    #                  '"$http_user_agent""$http_x_forwarded_for"';
 
    #access_log logs/access.log  main;
 
    sendfile        on; ＃允许发送文件
    #tcp_nopush     on;
 
    #keepalive_timeout  0;
    keepalive_timeout  65; ＃会话超时时间
 
    #gzip on;
 
    server {
        listen       80; ＃监听的端口
        server_name  localhost; ＃服务端域名或ip
 
        #charset koi8-r;
 
        #access_log  logs/host.access.log  main;
 
        location / { #Web服务的根目录
            root        /project/cwteam/cwteam/cwteam;
            index  index.html index.htm index.php;＃ 加入了html和php
            #如果文件不存在则尝试TP解析 
            try_files  $uri /index.php$uri;
        }
 
        error_page  404              /404.html;
 
        # redirect server error pages to thestatic page /50x.html
        #
        error_page   500 502 503 504  /50x.html; ＃可自定义错误页面
        location = /50x.html {
            root   html;
        }
 
        # proxy the PHP scripts to Apachelistening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #   proxy_pass   http://127.0.0.1;
        #}
 
        # pass the PHP scripts to FastCGIserver listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #   root          /project/cwteam/cwteam/cwteam;
        #   fastcgi_pass   127.0.0.1:9000;
        #   fastcgi_index  index.php;
        #   fastcgi_param SCRIPT_FILENAME /scripts$fastcgi_script_name;
        #   include        fastcgi_params;
        #}
        location ~ \.php {  ＃默认nginx不支持php拓展 这里把它添加上
            root        /project/cwteam/cwteam/cwteam; 
            fastcgi_pass   127.0.0.1:9000; 
            fastcgi_index  index.php; 
            fastcgi_intercept_errors on;
            #设置PATH_INFO，注意fastcgi_split_path_info已经自动改写了fastcgi_script_name变量， 
            #后面不需要再改写SCRIPT_FILENAME,SCRIPT_NAME环境变量，所以必须在加载fastcgi.conf之前设置 
            fastcgi_split_path_info  ^(.+\.php)(/.*)$; 
            fastcgi_param  PATH_INFO $fastcgi_path_info; 
             
            #加载Nginx默认"服务器环境变量"配置 
            include        fastcgi.conf; 
        } 
 
 
        # deny access to .htaccess files, ifApache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #   deny  all;
        #}
    }
 
 
    # another virtual host using mix of IP-,name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #   listen       somename:8080;
    #   server_name  somename  alias another.alias;
 
    #   location / {
    #       root   html;
    #       index  index.html index.htm;
    #   }
    #}
 
 
    # HTTPS server
    #
    #server {
    #   listen       443 ssl;
    #   server_name  localhost;
 
    #   ssl_certificate      cert.pem;
    #   ssl_certificate_key  cert.key;
 
    #   ssl_session_cache   shared:SSL:1m;
    #   ssl_session_timeout  5m;
 
    #   ssl_ciphers  HIGH:!aNULL:!MD5;
    #   ssl_prefer_server_ciphers  on;
 
    #   location / {
    #       root   html;
    #       index  index.html index.htm;
    #   }
    #}
 
}
```
