+++
title="git小记"
tags=["daily"]
categories=["日常"]
date="2020-10-12T09:00:00+08:00"
toc=true
+++

<!-- 概要 -->
GIT点点滴滴
<!--more-->

我们都知道git clone支持https和ssh协议, 之前一直以为一个public类型SSH协议仓库是不需要ssh gitkey的, 没想到今天却翻车了

## what

今天在使用`git clone`一个public类型的SSH协议仓库时, 却始终clone 失败, 让同事帮忙尝试克隆, 发现同事没问题, 百思不得其解。。。
```
git@github.com: Permission denied (publickey).
fatal: 无法读取远程仓库。

请确认您有正确的访问权限并且仓库存在。
```

## why

排查了自己的git配置, 没有什么特殊的配置:
+ /etc/ssh/ssh_config
+ ~/.ssh/config
+ ~/.gitconfig

后来听了一个同事想法, 才突然明白GIT SSH的工作原理:
+ 用户A`git clone`用户B的public类型SSH仓库:
1. 通过ssh鉴权github用户, 即默认通过公私钥`id和id.pub`认证github用户
2. 认证成功后, 此时用户A作为一个github用户方可`git clone`用户B的public类型SSH仓库


+ 用户A`git clone`用户B的 private类型SSH仓库:
1. 通过ssh鉴权github用户, 即默认通过公私钥`id和id.pub`认证github用户
2. 用户B上进行相关设置, 允许用户进行`git clone`相关操作, 比如加上用户的公钥等
3. 此时用户A作为一个github用户方可`git clone`用户B的 private类型SSH仓库

而自己之所以没有成功, 是因为自己的`git ssh key`没有用默认的公私钥`id和id.pub`, 从而导致一开始就没有进行github认证成功

## how

常见git命令
```shell
# 查看你当前的 remote url
git remote -v

# 调整远程url: SSH/HTTP
git remote set-url origin https://github.com/leesirc/leesirc.github.io.git
git remote set-url origin git@github.com:leesirc/leesirc.github.io.git

# debug git clone
GIT_SSH_COMMAND="ssh -vvv" git clone git@github.com:leesirc/leesirc-src.github.io.git

# git 相关参数查看
git config -l
```