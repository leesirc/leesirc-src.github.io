+++
title="ngx | 路由截取"
tags=["ngx"]
categories=["ngx"]
date="2020-10-16T09:00:00+08:00"
draft=false
toc=true
+++

<!-- 概要 -->
ngx url location
<!--more-->

## proxy_pass

```shell
location / {
    proxy_pass http://example.com:80/api
}

```

+ 总结

```shell
1. 只要proxy_pass server后有/, 不管是/api/xxx还是/api, 都会截断uri
2. 使用proxy_pass 方式截取都uri, 浏览器不变
3. 使用proxy_pass 方式都会将location 匹配到到前缀进行替换掉
```







## 参考链接
+ https://www.jianshu.com/p/849a6c068daa