---
title: "订阅转换(一)：subconverter加sub-web项目"
author: "Tom Almighty"
date: 2022-08-22
math: true
license: false
hidden: false
comments: true
draft: false
image: "https://pic.imgdb.cn/item/65f00c8c9f345e8d03e0d8b8.webp"
description: 通过 subconverter 和 sub-web 部署自己的订阅转换项目。
slug: "subconverter"
tags: ["订阅转换"]
categories: ["折腾记录"]
---

> 更新：网上找到的一键脚本，更加方便
```bash
# ubuntu
apt-get install -y wget && wget -O install.sh https://cdn.mxlong.com/install/subweb/install.sh && chmod +x ./install.sh && ./install.sh
# centos
yum install -y wget && wget -O install.sh https://cdn.mxlong.com/install/subweb/install.sh && chmod +x ./install.sh && ./install.sh
```
## **前言**

目前各个平台的代理软件的格式各不相同，各个服务厂商通常会提供给我们各个软件的订阅链接，但一些比较小的厂商提供的链接有限，或者提供的分流策略组不符合我们的预期，这个时候我们可以通过转换工具转换成我们所用软件的链接格式，或者使用自己修改的策略组实现更加灵活的分流。

## **准备工作**

- `VPS`一台
- 两个解析的域名，二级域名即可

**所用项目：**

前端：[sub-web](https://github.com/CareyWang/sub-web)

后端：[subconverter](https://github.com/tindy2013/subconverter)

## **步骤**

### **后端搭建**

1. 首先解析一个域名用作后端访问，这里以 `backend.domain.com` 为例，实际以自己的域名为准。
2. 连接VPS后，下载并解压后端程序，可以使用如下命令，也可以使用宝塔面板，下载程序后解压。

```bash
cd /opt # 这里是程序安装目录，可以自行更换
wget https://github.com/tindy2013/subconverter/releases/download/v0.8.1/subconverter_linux64.tar.gz
tar -zxvf subconverter_linux64.tar.gz
```

完成以后在 `/opt` 文件夹下会出现 `subconverter`文件夹，出现如下结构：

```bash
 .
 ├── subconverter
 │   ├── base
 │   ├── cache
 │   ├── config
 │   ├── generate.ini
 │   ├── gistconf.ini
 │   ├── pref.example.ini
 │   ├── pref.example.toml
 │   ├── pref.example.yml
 │   ├── pref.toml
 │   ├── profiles
 │   ├── rules
 │   ├── snippets
 │   └── subconverter
 └── subconverter_linux64.tar.gz
 
```

1. 修改相关配置参数

找到如下文件：

`/opt/subconverter/pref.toml`，修改下面两个参数：

```bash
api_access_token = "密码可以随意更改，也不用记住"  # 大概在第7行
managed_config_prefix = "https://backend.domain.com" # 改成解析的后端域名，大概在第149行
```

这里需要注意以下两点：

> 配置文件按照 `pref.toml pref.yml pref.ini` 的顺序加载第一次解压程序后会发现只有`pref.example.toml` 文件，这时候可以在程序所在目录 `/opt/subconverter` 手动运行一下程序：`./subconverter`，文件就会出现。
> 
1. 创建开机自启服务

在终端窗口输入如下命令，全部粘贴回车即可

```bash
cat > /etc/systemd/system/sub.service <<EOF
[Unit]
Description=Subconverter
After=network.target
 
[Service]
Type=simple
LimitAS=infinity
LimitRSS=infinity
LimitCORE=infinity
LimitNOFILE=65535
ExecStart=/opt/subconverter/subconverter
Restart=on-failure
RestartSec=10
 
[Install]
WantedBy=multi-user.target
EOF
```

> 如果修改了后端的运行目录，上边命令中第12行运行位置也要相应更改。
> 

设置开机自启：

```bash
systemctl enable sub # 开机自启
systemctl start sub # 启动
systemctl restart sub # 重启
systemctl status sub # 查看运行状态
```

到这里后端地址是 `http://127.0.0.1:25500/sub?`，由于上边修改了 `managed_config_prefix`，这时候我们需要反向代理。

1. 以后端域名新建一个站点

这里以宝塔为例，新建站点后打开设置，点击 `反向代理`，设置如下内容

![https://pic.imgdb.cn/item/62f5bb0716f2c2beb1f69f76.png](https://pic.imgdb.cn/item/62f5bb0716f2c2beb1f69f76.png)

保存后添加 `SSL`访问，至此后端搭建完成。

### **前端搭建**

1. 首先可以新建一个前端站点目录，无需数据库，新建后配置好 `SSL`，这里以 `web.domain.com` 为例。
2. 前端项目需要用到 `Node` 和 `Yarn` ，因此首先安装：

```bash
apt update && apt upgrade
apt install nodejs
apt install yarn
# 完成后使用如下命令查看是否安装成功
node -v
yarn --version
```

1. 下载 `sub-web`

命令下载到当前目录，默认为 `/root`，下载的位置可以自行选择，可以下载到前端站点目录中下（首先进入`/web.domain.com`后再执行如下命令）

```bash
git clone https://github.com/CareyWang/sub-web.git
cd sub-web
```

1. 安装依赖

```bash
yarn install
```

1. 修改后端配置及策略组文件

找到`/root/sub-web/src/views/Subconverter.vue` 文件，打开后在258行找到如下内容并修改，即将本地地址修改为后端地址，也可以添加多个后端地址。

```bash
 backendOptions: [{ value: "http://127.0.0.1:25500/sub?" }]
                              ↓
 backendOptions: [{ value: "https://backend.domain.com/sub?" }]
```

然后随后在下边的 `remoteConfig` 后修改策略组配置地址，可以根据文件的格式添加，这里给出 `ACL4SSR规则`配置，`remoteConfig[`后回车复制即可。

```bash
 {
             label: "ACL4SSR",
             options: [
               {
                 label: "ACL4SSR_Online 默认版 分组比较全 (与Github同步)",
                 value:
                   "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online.ini"
               },
               {
                 label: "ACL4SSR_Online_AdblockPlus 更多去广告 (与Github同步)",
                 value:
                   "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_AdblockPlus.ini"
               },
               {
                 label: "ACL4SSR_Online_NoAuto 无自动测速 (与Github同步)",
                 value:
                   "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_NoAuto.ini"
               },
               {
                 label: "ACL4SSR_Online_NoReject 无广告拦截规则 (与Github同步)",
                 value:
                   "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_NoReject.ini"
               },
               {
                 label: "ACL4SSR_Online_Mini 精简版 (与Github同步)",
                 value:
                   "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Mini.ini"
               },
               {
                 label: "ACL4SSR_Online_Mini_AdblockPlus.ini 精简版 更多去广告 (与Github同步)",
                 value:
                   "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Mini_AdblockPlus.ini"
               },
               {
                 label: "ACL4SSR_Online_Mini_NoAuto.ini 精简版 不带自动测速 (与Github同步)",
                 value:
                   "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Mini_NoAuto.ini"
               },
               {
                 label: "ACL4SSR_Online_Mini_Fallback.ini 精简版 带故障转移 (与Github同步)",
                 value:
                   "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Mini_Fallback.ini"
               },
               {
                 label: "ACL4SSR_Online_Mini_MultiMode.ini 精简版 自动测速、故障转移、负载均衡 (与Github同步)",
                 value:
                   "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Mini_MultiMode.ini"
               },
               {
                 label: "ACL4SSR_Online_Full 全分组 重度用户使用 (与Github同步)",
                 value:
                   "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Full.ini"
               },
               {
                 label: "ACL4SSR_Online_Full_NoAuto.ini 全分组 无自动测速 重度用户使用 (与Github同步)",
                 value:
                   "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Full_NoAuto.ini"
               },
               {
                 label: "ACL4SSR_Online_Full_AdblockPlus 全分组 重度用户使用 更多去广告 (与Github同步)",
                 value:
                   "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Full_AdblockPlus.ini"
               },
               {
                 label: "ACL4SSR_Online_Full_Netflix 全分组 重度用户使用 奈飞全量 (与Github同步)",
                 value:
                   "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Full_Netflix.ini"
               },
               {
                 label: "ACL4SSR 本地 默认版 分组比较全",
                 value: "config/ACL4SSR.ini"
               },
               {
                 label: "ACL4SSR_Mini 本地 精简版",
                 value: "config/ACL4SSR_Mini.ini"
               },
               {
                 label: "ACL4SSR_Mini_NoAuto.ini 本地 精简版+无自动测速",
                 value: "config/ACL4SSR_Mini_NoAuto.ini"
               },
               {
                 label: "ACL4SSR_Mini_Fallback.ini 本地 精简版+fallback",
                 value: "config/ACL4SSR_Mini_Fallback.ini"
               },
               {
                 label: "ACL4SSR_BackCN 本地 回国",
                 value: "config/ACL4SSR_BackCN.ini"
               },
               {
                 label: "ACL4SSR_NoApple 本地 无苹果分流",
                 value: "config/ACL4SSR_NoApple.ini"
               },
               {
                 label: "ACL4SSR_NoAuto 本地 无自动测速 ",
                 value: "config/ACL4SSR_NoAuto.ini"
               },
               {
                 label: "ACL4SSR_NoAuto_NoApple 本地 无自动测速&无苹果分流",
                 value: "config/ACL4SSR_NoAuto_NoApple.ini"
               },
               {
                 label: "ACL4SSR_NoMicrosoft 本地 无微软分流",
                 value: "config/ACL4SSR_NoMicrosoft.ini"
               },
               {
                 label: "ACL4SSR_WithGFW 本地 GFW列表",
                 value: "config/ACL4SSR_WithGFW.ini"
               }
             ]
           },
```

1. 打包网站

执行如下命令可打包网站：

```bash
yarn build
```

完成后会在 `/root/sub-web/`文件夹下生成一个 `dist`目录，这个目录就是网站的全部内容，可以将里面所有内容复制到前端站点的目录，如果是将前端下载到了站点目录而不是根目录，可以在宝塔面板中设置网站的运行目录，选择为 `/dist`即可。

![https://pic.imgdb.cn/item/62f5c5af16f2c2beb1146e2d.png](https://pic.imgdb.cn/item/62f5c5af16f2c2beb1146e2d.png)

> 如需要修改前端的配置，如加入更多的策略组配置文件，修改 `/root/sub-web/src/views/Subconverter.vue` 文件后，再在 `/root/sub-web` 目录下执行 `yarn build` 命令，随后再把 `/dist` 目录下内容替换到网站目录。
> 

至此前后端都已搭建完毕，打开前端地址即可正常使用。

## **参考**

- [参考地址1](https://www.v2rayssr.com/sub-web.html)
- [参考地址2](https://qingsay.com/subscription-converter.html)
- [参考地址3](https://www.mxlong.com/13.html)