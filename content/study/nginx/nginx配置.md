+++
title = "Nginx配置"
chaper = true
+++

# Nginx配置

## 1.nginx 关闭 开启

``` shell
#修改nginx.conf需要重启
#关闭nginx
nginx -s stop
#开启nginx
nginx
#重启nginx
nginx -s reload
#Win杀死所有nginx
taskkill /f /im nginx.exe
```

## 2.nginx 80端口解析多个网站

``` nginx
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       80;
        server_name hbk.hylink.net.cn;
    }
    
    server {
        listen       80;
        server_name hbk.applet.hylink.net.cn;
    }
}
```

## 3.nginx 开启ssl

``` nginx
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    
    server {
        listen       80;
        server_name 与443域名相同;
		rewrite ^/(.*)$ https网址域名:443/$1 permanent;
    }
    
    server {
        listen       443 ssl;
        server_name 域名;
        
		ssl_certificate      pem或crt文件地址;
		ssl_certificate_key  key文件地址;
		ssl_session_cache    shared:SSL:1m;
		ssl_session_timeout  5m;
		ssl_ciphers  HIGH:!aNULL:!MD5;
		ssl_prefer_server_ciphers  on;
    }
}
```

## 4.nginx开启gzip压缩

``` nginx
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    
	# 开启gzip压缩
    gzip on;
    # 不压缩临界值，大于1K的才压缩，一般不用改
    gzip_min_length 1k;
    # 压缩缓冲区
    gzip_buffers 16 64K;
    # 压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
    gzip_http_version 1.1;
    # 压缩级别，1-10，数字越大压缩的越好，时间也越长
    gzip_comp_level 5;
    # 进行压缩的文件类型
    gzip_types text/plain application/x-javascript text/css application/xml application/javascript;
    # 跟Squid等缓存服务有关，on的话会在Header里增加"Vary: Accept-Encoding"
    gzip_vary on;
    # IE6对Gzip不怎么友好，不给它Gzip了
    gzip_disable "MSIE [1-6]\.";
}
```

## 5.nginx 一个server代理多个目录

``` nginx
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    
    server {
        listen       80;
        server_name 域名;

    	location /a {
            root   目录地址;
        }
        
        location /b {
            alias   目录地址;
        }
    }
}
```

## 6.nginx反向代理网站

### 6.1nginx http镜像http

```nginx
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    
    server {
        listen 80;
        server_name 域名;
        charset utf-8;
        location / {
          proxy_pass 镜像的网址;
          proxy_set_header Accept-Encoding deflate;
          sub_filter_once off;
          sub_filter www.baidu.com mirror.example.com;
   
          proxy_redirect off;
          proxy_set_header        X-Real-IP       $remote_addr;
          proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
          proxy_max_temp_file_size 0;
          proxy_connect_timeout 90;
          proxy_send_timeout 90;
          proxy_read_timeout 90;
          proxy_buffer_size 4k;
          proxy_buffers 4 32k;
          proxy_busy_buffers_size 64k;
          proxy_temp_file_write_size 64k;
        }
  	}
}
```

### 6.2.nginx https镜像https

```nginx
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    
   	server {
        listen 443 ssl;
        server_name  域名;
		ssl_certificate      pem或crt文件地址;
		ssl_certificate_key  key文件地址;
		ssl_session_cache    shared:SSL:1m;
		ssl_session_timeout  5m;
		ssl_ciphers  HIGH:!aNULL:!MD5;
		ssl_prefer_server_ciphers  on;
        # 下面这段location配置是关键
        location / {
           sub_filter 需要镜像的网址 镜像后的网址;
           sub_filter_once off;
           proxy_ssl_session_reuse off;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header Referer 需要镜像的网址;
           proxy_set_header Host www.baidu.com;
           proxy_pass 需要镜像的网址;
           proxy_set_header Accept-Encoding "";
        }
    }
}
```

