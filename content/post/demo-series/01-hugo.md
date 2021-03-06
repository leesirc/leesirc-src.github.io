+++
title="hugo爬坑"
tags=["demo"]
categories=["demo"]
date="2020-10-11T09:00:00+08:00"
draft=false
toc=true
+++

<!-- 概要 -->
hugo-简单使用
<!--more-->

## 1.安装hugo脚手架

> 更多安装请参考:
>
> https://github.com/gohugoio/hugo
>
> https://gohugo.io/getting-started/installing/

### 1.1 Binary Install

```shell
# Homebrew (macOS)
brew install hugo

# 验证安装
hugo version
```

### 1.2 Advanced Install

```shell
mkdir $HOME/src
cd $HOME/src
git clone https://github.com/gohugoio/hugo.git
cd hugo
go install

# 验证安装
hugo version
```


## 2.初始化站点

### 2.1 初始化blog

```shell
# 使用hugo脚手架创建新站点, 此处我的站点名称为blog
hugo new site blog

cd blog/
git init
# 使用新的Hugo maupassant主题, 这款真心不错, 强烈推荐哦
git submodule add https://github.com/rujews/maupassant-hugo themes/maupassant
```

### 2.2 初始化设置

```shell
# 主题的更多设置请参考下面的github地址
# 每款主题都有demo, 请详细查看哈
https://github.com/flysnow-org/maupassant-hugo
```



### 2.3 发布文章

```shell
# 进入站点根目录
hugo new posts/demo.md

# 本地测试, http://localhost:1313/
hugo server -D
```

### 2.4 常用配置

#### Front Matter
markdown 文件开头的 --- 内的内容称为 Front Matter，用于文章生成的一些控制，Hugo 支持TOML、YAML、JSON格式的Front Matter，下面节选些常用的配置：
```yaml

---
title: "文章标题"
description: "文章的描述信息"
# 建议对标签的理解为：文章所涉及到的技术、内容点，越多越好
tags: [ "标签1", "标签2"]
# 建议对目录的理解为：文章所属的系列，越少越好
categories: [ "分类1", "分类2" ]
# 关键词用于 HTML head 的 keyword
keywords: [ "Hugo", "blog" ]
date: 2019-01-01
lastmod: 2019-1-1
# CJKLanguage: Chinese, Japanese, Korean
isCJKLanguage: true

# 如果draft为true，除非使用 --buildDrafts 参数，否则不会发布文章
draft: false

# 设置文章的过期时间，已过期的文章不会发布，除非使用 --buildExpired 参数
expiryDate: 2020-01-01

# 设置文章的发布时间，未来的文章不会发布，除非使用 --buildFuture 参数
publishDate: 2020-01-01

# 文章排序权重
weight: 40

# 还有些有些第三方自定义的
comment: true
hiddenFromHomePage: false
contentCopyright: true
reward: false
---
```

## 3.初始化github

### 3.1 创建github仓库

```shell

# 仓库名称必须是<name>.github.io
此处我的仓库名称是leesirc.github.io
```

### 3.2 上传相关代码

```shell
git submodule init
git submodule update
 
hugo --buildDrafts
cd public/
git init
git remote rm origin 
git remote add origin https://github.com/leesirc/leesirc.github.io.git
git add -A
git commit -m "init"
git push -u origin master
```

## 4.文章更新

```shell
# 1.本地预览测试
cd <站点根目录>
hugo server -D

# 2.文章发布github page
cd <站点根目录>
hugo -D
cd public
git add . && git commit -m 'update site' && git push -v

```

## 5.总结
```shell
1. 此次分为3个仓库: 博客源码系统-private, 博客静态文件系统-public, 博客评论仓库-public
2. 博客评论仓库采用的是utteranc评论, 默认会变成issue到博客评论仓库里
```

## 参考链接

+ https://github.com/flysnow-org/maupassant-hugo
+ https://olowolo.com/post/hugo-quick-start/
+ https://corpython.github.io/
+ https://zhuanlan.zhihu.com/p/105021100?utm_source=weibo
+ http://www.flysnow.org/2018/07/29/from-hexo-to-hugo.html
+ http://wangtingkui.com/categories/%E4%BD%BF%E7%94%A8hugo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E7%BD%91%E7%AB%99/
+ etc