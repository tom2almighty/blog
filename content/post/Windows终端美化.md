---
title: "Windows终端美化"
author: "Tom Almighty"
date: 2024-03-18T10:27:32+08:00
math: true
license: false
hidden: false
comments: true
draft: false
image: "https://pic.imgdb.cn/item/65f7c4779f345e8d03321bad.webp"
description: 安装 oh-my-posh 美化 Windows 终端 Windows Terminal
slug: "windows-terminal"
tags: ["Windows","终端"]
categories: ["折腾记录"]
---

**前言：**Windows 推出了新的终端 `Windows Teminal`，虽然外观有所进步，但还是达不到自己预期，因此可以自己简单修改一下。

## 准备工作

计算机上启动 `Windows PowerShell` 时，执行策略很可能是 `Restricted`（默认设置）。

- `Restricted` 执行策略不允许任何脚本运行。 
- `AllSigned` 和 `RemoteSigned` 执行策略可防止 `Windows PowerShell` 运行没有数字签名的脚本。

因此如果直接安装很可能出现无法运行脚本的错误。

### 查看当前策略

打开 `PowerShell` 然后输入:

```powershell
get-executionpolicy

PS C:\WINDOWS\system32> get-executionpolicy
Restricted
```

### 修改策略

以管理员身份打开 `Windows Posershell`，执行下列命令：

```powershell
set-executionpolicy remotesigned
# 运行后输入 Y
```



## 安装

1. 首先在微软商店下载两个应用，直接搜索 `Windows Terminal` 以及 `oh-my-posh`。

2. 安装字体。安装作者推荐的 `MesloLGM NF` 字体，[点此下载](https://github.com/ryanoasis/nerd-fonts/releases/download/v2.1.0/Meslo.zip)，这样可以解决部分字体图标渲染问题。

3. 设置字体。打开 `Windows Terminal`，按 `Ctrl + Shift + 逗号`打开设置文件，然后在第 `41` 行 `profiles` 下 `default` 中添加字体设置，在第 `43` 行回车，然后添加如下代码后保存。
   ```json
   "font":
   {
       "face": "MesloLGM NF"
   }
   ```

4. 加载 `oh-my-posh` 设置，在 `Windows Terminal` 中输入下面第一行命令，提示创建新文件，创建后输入第二行命令保存：
   ```powershell
   notepad $profile
   oh-my-posh init pwsh | Invoke-Expression
   ```

   这样 `oh-my-posh` 就安装完成了，接下来可以自己选择主题。

## 主题

`oh-my-posh` 有很多主题，可以在官方文档页面预览，地址如下：

[主题预览](https://ohmyposh.dev/docs/themes)

选择好自己的主题后，在终端输入下面的命令：

```powershell
Get-PoshThemes
```

然后终端会出现所有的主题预览，最后会出现主题文件所在地址，打开即可看到所有的主题文件，再次输入 `notepad $profile`，然后将配置修改为以下：

```powershell
oh-my-posh init pwsh --config $env:POSH_THEMES_PATH/cloud-native-azure.omp.json | Invoke-Expression
```

可以将主题名称修改为自己想要的主题，保存后即可完成。

## 配置自动补全

自动补全插件为 [PSReadLine](https://github.com/PowerShell/PSReadLine)

在终端安装模块：

```powershell
Install-Module -Name PowerShellGet -Force -Scope CurrentUser
Install-Module PSReadLine -Scope CurrentUser
```

> 或者可以使用管理员打开终端，这样命令后就不需要加 `-Scope CurrentUser`

安装好模块后输入 `notepad $profile`打开配置文件，添加下列命令：

```powershell
Import-Module PSReadLine
Set-PSReadlineKeyHandler -Key Tab -Function Complete
```

或者可以直接在配置文件中添加官方示例的代码，地址[点击这里](https://raw.githubusercontent.com/PowerShell/PSReadLine/master/PSReadLine/SamplePSReadLineProfile.ps1)，需要注意的是，下面两行代码需要放到最开头，同时在 `Import-Module PSReadLine` 下一行添加 `Set-PSReadlineKeyHandler -Key Tab -Function Complete`。

```powershell
# 放到开头
using namespace System.Management.Automation
using namespace System.Management.Automation.Language
...
Import-Module PSReadLine
Set-PSReadlineKeyHandler -Key Tab -Function Complete

```

## 个人配置

```powershell
# 初始化配置及主题
oh-my-posh init pwsh --config $env:POSH_THEMES_PATH/cloud-native-azure.omp.json | Invoke-Expression
# 导入模块
Import-Module PSReadLine
# 设置 TAB 键补全
Set-PSReadlineKeyHandler -Key Tab -Function Complete
# 设置 Ctrl+z 为撤销
Set-PSReadLineKeyHandler -Key "Ctrl+z" -Function Undo
# 设置向上键为后向搜索历史记录
Set-PSReadLineKeyHandler -Key UpArrow -ScriptBlock {
  [Microsoft.PowerShell.PSConsoleReadLine]::HistorySearchBackward()
  [Microsoft.PowerShell.PSConsoleReadLine]::EndOfLine()
}
 
# 设置向下键为前向搜索历史纪录
Set-PSReadLineKeyHandler -Key DownArrow -ScriptBlock {
  [Microsoft.PowerShell.PSConsoleReadLine]::HistorySearchForward()
  [Microsoft.PowerShell.PSConsoleReadLine]::EndOfLine()
}
```



## 参考

- [syxdevcode 博客](https://syxdevcode.github.io/2021/09/04/PowerShell%EF%BC%9A%E5%9B%A0%E4%B8%BA%E5%9C%A8%E6%AD%A4%E7%B3%BB%E7%BB%9F%E4%B8%8A%E7%A6%81%E6%AD%A2%E8%BF%90%E8%A1%8C%E8%84%9A%E6%9C%AC%EF%BC%8C%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95/)

- [oh-my-posh 文档](https://ohmyposh.dev/docs/themes)

- [ChrAlpha's Blog](https://blog.ichr.me/post/windows-terminal-configuration/#%E6%9B%B4%E8%BF%9B%E4%B8%80%E6%AD%A5%E2%80%94%E2%80%94Oh-My-Posh)

- [知乎](https://zhuanlan.zhihu.com/p/354603010)
- [志文工作室](https://lzw.me/a/windows-terminal-update-oh-my-posh.html/comment-page-1)