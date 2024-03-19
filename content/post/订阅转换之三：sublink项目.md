---
title: "订阅转换之三：sublink项目"
author: "Tom Almighty"
date: 2024-03-06
math: true
license: false
hidden: false
comments: true
draft: false
series: ["订阅管理"]
image: "https://pic.imgdb.cn/item/65f00c8c9f345e8d03e0d8b8.webp"
description: 通过 docker 部署订阅管理系统，快捷管理自建节点。
slug: "sublink"
tags: ["订阅转换","Dokcer",""]
categories: ["折腾记录"]
---

> 😀 自建的节点往往难以管理，可以通过搭建 `v2b` 等面板管理，但难免繁琐，这里提供另一种方式。


项目地址：[sublink](https://github.com/jaaksii/sublink)

## 安装 docker 和 docker-compose

```bash
# 卸载旧版本
sudo apt-get remove docker \
             docker-engine \
             docker.io
# 安装 docker
curl -fsSL get.docker.com -o get-docker.sh
# 启动并设置开机自启
sudo systemctl enable docker
sudo systemctl start docker
# 安装 docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
# 可用以下命令测试安装
docker-compose --version
```

或者通过下面脚本安装：

```bash
# kejilion 工具箱
curl -sS -O https://raw.githubusercontent.com/kejilion/sh/main/kejilion.sh && chmod +x kejilion.sh && ./kejilion.sh
```

## 部署程序

```bash
cd /opt && mkdir sublink && cd sublink
wget https://raw.githubusercontent.com/jaaksii/sublink/master/docker-compose.yml
docker-compose up -d
```

之后可创建网站，反向代理 `http://127.0.0.1:8000` 。

## 参考文章

- [项目地址](https://github.com/jaaksii/sublink)