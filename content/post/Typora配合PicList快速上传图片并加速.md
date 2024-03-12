---
title: "Typora配合PicList快速上传图片并加速 "
author: "Tom Almighty"
date: 2023-08-03
math: true
license: false
hidden: false
comments: true
draft: false
image: "https://pic.imgdb.cn/item/65dc41809f345e8d03fcc5fb.webp"
description: 通过PicList快速上传笔记图片。
slug: "piclist"
tags: ["Typora","Piclist","图床","Github"]
categories: ["折腾记录"]
---
**前言：**在Typora写笔记时不免使用图片，这里使用GitHub作图床，配合PicList上传，并通过jsdelivr实现CDN加速。

## **一. 准备**

首先在 GitHub 中创建一个新的公共仓库。接下来生成一个新的 Token，地址为[https://github.com/settings/tokens](https://github.com/settings/tokens)， Token 权限需要勾选`write:packages`和`read:packages`

> Token 只显示一次，忘记了需要重新生成，注意保存
> 

## **二. 创建网站**

1. 在服务器上创建一个新的网站和数据库，将下面项目打包后放入网站根目录并解压。

[autoPicCDN](https://github.com/yumusb/autoPicCdn)

1. 将项目中的数据库文件`pic.sql`上传到数据库中并替换覆盖

## **三. 修改参数**

在 `up.php` 中修改参数：

- 一般选择 GitHub
- USER 填写用户名
- REPO 填写仓库名
- TOKEN 填写第一步的TOKEN
- 数据库信息只需修改 `dbname` `user` `pass`，与第二步中一致

![https://pic.imgdb.cn/item/64cb9b6d1ddac507cc346d67.png](https://pic.imgdb.cn/item/64cb9b6d1ddac507cc346d67.png)

## **四. 配置 PicList**

1. 下载 PicList（PicGo基础上开发） 或者 Picgo，地址如下：

[PicList](https://github.com/Kuingsmile/PicList)

[PicGo](https://github.com/Molunerfinn/PicGo)

1. 安装 `web-uploader`插件
2. 配置上传参数，填好后确认
    1. API 地址填写`up.php`路径，如`www.pic.com/up.php`
    2. POST 参数名填写 `pic`
    3. JSON 路径填写 `data.url`

![https://pic.imgdb.cn/item/64cb9b641ddac507cc3450c6.png](https://pic.imgdb.cn/item/64cb9b641ddac507cc3450c6.png)

## **五. 配置 Typora**

1. 依次打开`文件->偏好设置->图像`
2. `插入图片时`选择“上传图片”
3. 在最下方`上传服务设定`选择对应的应用 `PicList`或者 `PicGo`