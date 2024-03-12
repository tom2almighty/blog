---
title: "Docker部署订阅管理系统"
author: "Tom Almighty"
date: 2024-03-06
math: true
license: false
hidden: false
comments: true
draft: false
image: "https://pic.imgdb.cn/item/65f00c8c9f345e8d03e0d8b8.webp"
description: 通过 docker 部署订阅管理系统，快捷管理自建节点。
slug: "sublink"
tags: ["订阅转换"]
categories: ["折腾记录"]
---

> 😀 自建的节点往往难以管理，可以通过搭建 `v2b` 等面板管理，但难免繁琐，这里提供另一种方式。


项目地址：[sublink](https://github.com/jaaksii/sublink)

# 安装 docker

```bash
# 卸载旧版本
sudo apt-get remove docker \
             docker-engine \
             docker.io
curl -fsSL get.docker.com -o get-docker.sh
# 启动并设置开机自启
sudo systemctl enable docker
sudo systemctl start docker
```

```bash
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

# 部署程序

下面两种方式选择一种即可，推荐第二种。

## docker cli

```bash
docker pull jaaksi/sublink
docker volume create sublink_data
docker run --name sublink -p 8000:5000 \
-v sublink_data:/app/app/db \
-e PORT=5000 \
-d jaaksi/sublink
```

## docker-compose

```bash
cd /opt
mkdir sublink
cd sublink
touch docker-compose.yml
vim docker-compose.yml
```

粘贴如下内容：

```yaml
version: '3.8'
services:
  sublink:
    image: jaaksi/sublink
    container_name: sublink
    ports:
      - "8000:5000"
    volumes:
      - sublink_data:/app/app/db
    environment:
      - PORT=5000
    restart: always

volumes:
  sublink_data:

```

完成后输入 `:wq` 保存退出，之后运行容器：

```docker
docker-compose up -d
```

之后可创建网站，反向代理 `http://127.0.0.1:8000` 。

# 📎 参考文章

- [项目地址](https://github.com/jaaksii/sublink)