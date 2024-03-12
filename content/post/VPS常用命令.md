---
title: "VPS常用命令"
author: "Tom Almighty"
description: VPS常用命令记录。
date: 2020-08-17
image: "https://pic.imgdb.cn/item/65dc47829f345e8d030c9c98.webp"
math: true
license: false
hidden: false
comments: true
draft: false
slug: "vps"
tags: ["VPS"]
categories: ["折腾记录"]
---
## **宝塔面板**

```bash
# centos
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
 
# ubuntu/deepin
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh
 
# debian
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh
 
# fedora
wget -O install.sh http://download.bt.cn/install/install_6.0.sh && bash install.sh
 
# 更新
curl http://download.bt.cn/install/update6.sh|bash
 
# 删除登录验证(适用于 7.8.0 以前版本)
sed -i "s|if (bind_user == 'True') {|if (bind_user == 'REMOVED') {|g" /www/server/panel/BTPanel/static/js/index.js
rm -rf /www/server/panel/data/bind.pl
# 还原绑定
sed -i "s|if (bind_user == 'REMOVED') {|if (bind_user == 'True') {|g" /www/server/panel/BTPanel/static/js/index.js
# 开心版
wget -O install.sh http://download.bt.sy/ltd/install/install-ubuntu_6.0.sh && sudo bash install.sh
```

## **BBR 加速**

```bash
# 国内版
 
wget "https://dt.yixiagege.cn/shell/TCP-CN.sh" && chmod +x TCP-CN.sh && ./TCP-CN.sh
```

## **测速**

```bash
# SuperSpeed
 
bash <(curl -Lso- https://git.io/J1SEh)
 
# LemonBench
 
curl -fsL https://ilemonra.in/LemonBenchIntl | bash -s fast
```

## **更改 SSH 端口**

```bash
# 备份
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
# 更改端口
vim /etc/ssh/sshd_config
# 重启服务
systemctl restart sshd
# 设置防火墙
# firewall
firewall-cmd --permanent --zone=public --add-port=xxxx/tcp
firewall-cmd --reload
# iptables
iptables -A INPUT -p tcp --dport xxxx -j ACCEPT
service iptables save
systemctl enable iptables
```

## **一键安装 File2Ban**

```bash
 # 安装
wget https://raw.githubusercontent.com/FunctionClub/Fail2ban/master/fail2ban.sh
bash fail2ban.sh
# 卸载
wget https://raw.githubusercontent.com/FunctionClub/Fail2ban/master/uninstall.sh
bash uninstall.sh
```

## **Brook 中转**

```bash
wget -N --no-check-certificate https://zhujiget.com/wp-content/uploads/2020/brook-pf.sh && chmod +x brook-pf.sh && ./brook-pf.sh
```

## **添加 Swap 分区**

```bash
wget https://www.moerats.com/usr/shell/swap.sh && bash swap.sh
```

## **buyvm 挂载存储块**

```bash
# 查询存储块ID
ls /dev/disk/by-id/
# 格式化硬盘
mkfs.ext4 -F /dev/disk/by-id/scsi-0BUYVM_SLAB_VOLUME-xxxxx
# 创建挂载点
mkdir /mnt/pan
# 挂载硬盘
mount -o discard,defaults /dev/disk/by-id/scsi-0BUYVM_SLAB_VOLUME-xxxxx /mnt/pan
# 开机自动挂载
echo '/dev/disk/by-id/scsi-0BUYVM_SLAB_VOLUME-xxxxx /mnt/pan ext4 defaults,nofail,discard 0 0' | sudo tee -a /etc/fstab
```

## 流媒体解锁查询

```bash
# 原版
bash <(curl -L -s check.unlock.media)
# 添加解锁方式查询
bash <(curl -L -s media.ispvps.com)
```

## **Azure 使用 root 登录**

```bash
echo root:你的密码 |sudo chpasswd root
sudo sed -i 's/^.*PermitRootLogin.*/PermitRootLogin yes/g' /etc/ssh/sshd_config;
sudo sed -i 's/^.*PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config;
sudo service sshd restart
```

## 工具箱

```bash
# 科技 Lion
curl -sS -O https://raw.githubusercontent.com/kejilion/sh/main/kejilion.sh && chmod +x kejilion.sh && ./kejilion.sh
# Bluesky
wget -O box.sh https://raw.githubusercontent.com/BlueSkyXN/SKY-BOX/main/box.sh && chmod +x box.sh && clear && ./box.sh
```