title: 关于http和https允许请求设置header问题
author: TryCatch
tags:
  - https
  - nginx
categories:
  - https
date: 2017-05-17 14:35:00
---
在给微信小程序提供接口的时候使用的是```https```，```token```是放在header里面， 
不过服务器一直没获取到，怀疑可能是```nginx```没有允许设置```header```。解决办法就是在```nginx```配置文件里面加上```underscores_in_headers on```

在给微信小程序提供接口的时候使用的是https，token是放在header里面， 
不过服务器一直没获取到，怀疑可能是nginx没有允许设置header。解决办法就是在nginx配置文件里面加上underscores_in_headers on;
```
 ### wechatapi Start
  upstream wechatapi {
    server  localhost:8009;
  }
  server {
      listen  443 ssl;
      server_name api.test.org;
      underscores_in_headers on;
      ssl on;
      ssl_certificate /ssl/bundle.crt;
      ssl_certificate_key /ssl/txzs_unsecure.key;
      client_max_body_size 20M;
      access_log /system/logs/wechatapi_access.log;
      error_log  /system/logs/wechatapi_error.log;
      root       /system/wechatapi/current/public;
      index      index.html;

      location / {
        try_files $uri @wechatapi;
      }

      location @wechatapi {
        proxy_read_timeout 300;
        proxy_connect_timeout 300;
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_pass http://wechatapi;
      }

      error_page 403 /500.html;
      error_page 404 /500.html;
      error_page 405 /500.html;
      error_page 500 501 502 503 504 /500.html;

      location ^~ /error/ {
          internal;
          root /system/wechatapi/current/public;
      }

      location /images/ {
        try_files $uri /images/error.jpg;
      }

  }
  # wechatapi end
```