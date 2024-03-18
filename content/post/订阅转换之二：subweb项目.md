---
title: "订阅转换之二：subweb项目"
author: "Tom Almighty"
date: 2024-01-27
math: true
license: false
hidden: false
comments: true
draft: false
series: ["订阅转换"]
image: "https://pic.imgdb.cn/item/65f00c8c9f345e8d03e0d8b8.webp"
description: Docker compose 一键部署订阅转换前后端加短链
slug: "sub-web"
tags: ["订阅转换","Docker"]
categories: ["折腾记录"]
---
## 前言：

整理服务器时发现部署的 `Subconverter` 后端一直出问题，无法正确转换，经过搜索发现了一个前后端加短链的 `Docker` 整合版，相比较原项目前后端，有如下优点：

- `Docker Compose`部署十分方便
- 前端界面美观
- 远程配置和自定义后端选择框比较明显

https://github.com/stilleshan/subweb

## 部署教程

### 1. 安装 `Docker`

```bash
curl -sSL https://get.docker.com | bash
systemctl enable docker
systemctl start docker
```

### 2. 安装 `Docker compose`

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
# 测试安装
docker-compose --version
```

### 3. 部署网站

部署两个网站，并配置好证书。一个用作短链，一个用作订阅转换前后端。

### 4. 下载 `compose` 文件

```bash
cd ~
mkdir subconverter
cd subconverter
wget https://raw.githubusercontent.com/stilleshan/dockerfiles/main/sub/docker-compose.yml
```

### 5. 修改配置

将下载的 `compose`文件中第 `16` 行的域名改为自己的短链

### 6. 部署程序

```bash
cd /root/subconverter
docker-compose up -d
```

完成后文件夹下会多出两个文件夹，`/data & /conf`，在 `conf/config.js`文件中修改对应的地址：

```bash
 apiUrl: '<http://web.sub.com>', # 订阅转换
 shortUrl: '<https://a.cc>', # 短链
 # 13行的短链地址也要更换
```

## 7. 反向代理

订阅转换前后端的反向代理目标 `url`：

```bash
# 订阅转换前后端的目标url
http://127.0.0.1:18080
# 短链反向代理配置(可直接添加到配置文件)：
        location / {
        proxy_redirect off;
        proxy_pass http://127.0.0.1:8002;

        add_header 'Access-Control-Allow-Origin' '*';

        proxy_set_header  Host                $http_host;
        proxy_set_header  X-Real-IP           $remote_addr;
        proxy_set_header  X-Forwarded-Ssl     on;
        proxy_set_header  X-Forwarded-For     $proxy_add_x_forwarded_for;
        proxy_set_header  X-Forwarded-Proto   $scheme;
        proxy_set_header  X-Frame-Options     SAMEORIGIN;

        client_max_body_size        100m;
        client_body_buffer_size     128k;
    }

```

> 💡 如果不需要短链服务，只需要在 `compose` 文件中删除第 `12-32` 行,将默认使用作者短链接，也可以修改 `conf/config.js` 使用其他短链接服务。


## 参考

- [stilleshan/subweb](https://github.com/stilleshan/subweb)