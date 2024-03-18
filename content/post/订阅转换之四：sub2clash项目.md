---
title: "订阅转换之四：sub2clash项目"
author: "Tom Almighty"
date: 2024-03-18T14:01:34+08:00
math: true
license: false
hidden: false
comments: true
draft: false
series: ["订阅转换"]
image: "https://pic.imgdb.cn/item/65f00c8c9f345e8d03e0d8b8.webp"
description: 将节点和订阅快速转换为 clash (meta) 配置
slug: "sub2clash"
tags: ["订阅转换","Docker"]
categories: ["折腾记录"]
---

**前言：** 部分订阅转换修改节点后无法使用相同的链接，本次的项目可以方便地添加最新协议节点，同时可以灵活添加国家和地区分组，并且可以添加自定义规则，也可以使用自定义短链接。

本次项目地址如下：

[sub2clash](https://github.com/nitezs/sub2clash)

## 安装

```bash
mkdir -p /opt/sub2clash && cd /opt/sub2clash
wget https://raw.githubusercontent.com/nitezs/sub2clash/main/docker-compose.yml
docker-compose up -d
```

可以自己修改 `docker compose` 的环境变量，可修改参数如下：

| 变量名                | 说明                                           | 默认值                |
| --------------------- | ---------------------------------------------- | --------------------- |
| PORT                  | 端口                                           | `8011`                |
| META_TEMPLATE         | 默认 meta 模板文件名                           | `template_meta.yaml`  |
| CLASH_TEMPLATE        | 默认 clash 模板文件名                          | `template_clash.yaml` |
| REQUEST_RETRY_TIMES   | Get 请求重试次数                               | `3`                   |
| REQUEST_MAX_FILE_SIZE | Get 请求订阅文件最大大小（byte）               | `1048576`             |
| CACHE_EXPIRE          | 订阅缓存时间（秒）                             | `300`                 |
| LOG_LEVEL             | 日志等级，可选值 `debug`,`info`,`warn`,`error` | `info`                |
| SHORT_LINK_LENGTH     | 短链长度                                       | `6`                   |

## 模板

模板的设置可以参考默认，也可以自行设置：

- `<all>` 为添加所有节点
- `<countries>` 为添加所有国家策略组
- `<地区二位字母代码>` 为添加指定地区所有节点，例如 `<hk>` 将添加所有香港节点

meta 配置也可以参考内核文档的推荐配置：[虚空终端](https://wiki.metacubex.one/example/conf/#__tabbed_1_1)，`GEOSITE`目录可以参考 [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community/tree/master/data)

## 参考

- [sub2clash](https://github.com/nitezs/sub2clash)