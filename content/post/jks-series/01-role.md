+++
title="jks | role"
tags=["jks"]
categories=["Jenkins"]
date="2020-10-16T09:00:00+08:00"
draft=false
toc=true
+++

<!-- 概要 -->
Jenkins role
<!--more-->

+ jenkins有两种role：global role 和 project role
```shell
1. manage role
global role是全局的role：admin，dev，manager等等，应该是设置jks全局的角色
project role是具体的job role：可以为相同类型的job进行分组, 并设置相应的权限

2. assign role
golabal role，可以增加ldap对应的用户组或者user，进行授权jks全局的角色
item role，可以授权ldap对应的用户组或者用户 相应的project rol
```