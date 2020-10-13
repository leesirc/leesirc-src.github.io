+++
title="阿里云数据湖"
tags=["aliyun"]
categories=["阿里云"]
date="2020-10-13T09:00:00+08:00"
draft=false
toc=false
+++

<!-- 概要 -->
阿里云数据湖小记
<!--more-->

最近简单看了下阿里云的数据湖产品:
DLA产品的账号有ram子账号和DLA子账号, 前者是spark引擎用到, 后者是SQL引擎用到
+ https://help.aliyun.com/document_detail/181103.html?spm=a2c4g.11186623.6.753.3daf303eXjGPNY

```shell
1. 创建角色: role, arn是role id的标示, 涉及授信服务和访问哪些资源
2. 授权ram子用户: 首先通过绑定arn创建自定义策略, 然后将该策略授权为ram子用户
```