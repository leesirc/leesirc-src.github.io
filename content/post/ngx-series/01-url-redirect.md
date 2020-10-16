+++
title="ngx | 路由跳转"
tags=["ngx"]
categories=["ngx"]
date="2020-10-16T09:00:00+08:00"
draft=false
toc=true
+++

<!-- 概要 -->
ngx url重定向实践
<!--more-->

路由跳转有好几种方式实现

## rewrite

```shell
server {
    listen       80;
    server_name  example.com;
    return       301 https://www.example.com$request_uri;
}

server {
    listen       443;
    server_name  www.example.com;
    return       301 https://www.example.com$request_uri;

    ...
}

server {
    listen       443 ssl;
    server_name  www.example.com;
    ssl on;
    ssl_certificate      ssl/www.example.com.crt;
    ssl_certificate_key  ssl/www.example.com.key;
    ...
}

```









## 参考链接
+ https://www.jianshu.com/p/aa0846c9c650