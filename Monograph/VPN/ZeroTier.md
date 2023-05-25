---
title: ZeroTier
author: QinMo
date: 2023-04-26 23:30:09
modified_date: 2023-05-22 22:37:11 
tags: Document VPN
alias: 
---
# ZeroTier

> ZeroTier 可让您构建几乎任何类型的现代化、安全的多点虚拟化网络。从强大的对等网络到多云网状基础设施，我们通过简单的本地网络实现全球连接。

简单来说，这就是一款组建虚拟局域网的软件。

## 安装 Zero

> 下载地址 [Download (zerotier.com)](https://www.zerotier.com/download/)

linux 平台使用 [[#安装 zerotier-one]] 方法。

## 创建网络

在 zeroTier 平台上操作
1. 创建好自己的私有网络
2. 配置网段地址

## 加入网络
windows 平台加入网络，右键 zeroTier 状态图标，Join New network -> 输入网络 Id。

Linu 上加入网络
```Shell
zerotier-cli join [networkId]
```


# Zerotier Moon
通过在Zerotier Network中建立的虚拟局域网进行访问时，zertier one建立加密通道，网络环境优良时能够通过Udp穿透建立通畅的隧道，但在NAT层数以及其他网络限制的情况下这样的隧道无法建立🚫，此次zerotier的传输策略将会变成由zertier官方的服务器做流量转发。

![[Drawing 2023-04-26 15.21.36.excalidraw]]

由于zerotier的转发服务器在海外以及GFW的存在，通过zeritier进行中转，效果会很不理想。

![[Drawing 2023-04-26 15.58.32.excalidraw]]

Zerotier Moon的作用意在让能够在无法穿透的网络环境中使用自建的中转服务器，达到较好的网络质量。当官方中转不够顺畅时，✅能够选择最优的Moon服务器。
![[Drawing 2023-04-26 16.28.06.excalidraw]]

# Moon Server Install

## 安装 zerotier-one
```bash
curl -s https://install.zerotier.com | sudo bash
```

## 配置Moon

进入zerotier-one目录
```bash
cd /var/lib/zerotier-one
```

生成moon.json 文件
> [!info]
>❗注意一下命令都需要 root 用户执行,或 root 权限


```Bash
zerotier-idtool initmoon identity.public >> moon.json
```

编辑moon.josn文件
```bash
vim moon.json
```

> [!info]
> 修改(这里一定要带""还有端口一定要用/


```json
//只修改"stableEndpoints": ["服务器公网IP/9993"]
"stableEndpoints": ["x.x.x.x/9993"]
```

## 生成.moon文件
```bash
zerotier-idtool genmoon moon.json
```

如果当前目录下不存在`moons.d` 目录,新建该目录
```bash
mkdir ./moons.d
```

将生成的 000000xxxxxxxxxx.moon 移动到 `moons.d` 目录
```bash
mv 000000xxxxxxxxxx.moon moons.d
```

## 重启 zerotier-one 服务
```bash
systemctl restart zerotier-one
```

# 客户端使用Moon服务器

### 自动配置
- Linux
```bash
sudo zerotier-cli orbit [moon.json文件中的id] [moon.json文件中的id]
```
- Windows
> [!info]
> 需要使用管理员权限的 PowerShell 输入


```PowerShell
zerotier-cli orbit [moon.json文件中的id] [moon.json文件中的id]
```

### 手动配置
在Moon服务器中生成的.moon文件记得拷贝下来可以用作手动配置
- Linux
```bash
# 创建文件夹
mkdir /var/lib/zerotier-one/moon.d
# 移动moon.d文件到文件夹下
mv 000000xxxxxxxxxx.moon /var/lib/zerotier-one/moons.d
```
- windows
```PowerShell
# 创建文件夹
mkdir C:\ProgramData\ZeroTier\One\moon.d
# 移动moon.d文件到文件夹下
mv 000000xxxxxxxxxx.moon C:\ProgramData\ZeroTier\One\moons.d
```

### 重启服务
- Linux
```bash
/etc/init.d/zerotier-one restart
```
- windows
-   按下 windows键+r ，打开 “运行” 窗口。
-   输入 services.msc 回车。
-   找到 ZeroTier One 服务，右键选择 “重新启动” 。

### 检测生效
在非 moon 的客户端，输入命令：
- Linux
```bash
zerotier-cli listpeers
```
- windows
```PowerShell
zerotier-cli listpeers
```

如果出现如下情况：

 moon 服务器的 ID 、IP 地址出现在列表中，证明联通 moon 服务器。
```txt
200 listpeers <ztaddr> <path> <latency> <version> <role> 
................... 
200 listpeers 6xxxxxxxxx [moon IPv4地址]/60723;11450;11405 -1 1.4.6 MOON 
...................
```