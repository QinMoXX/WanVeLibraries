---
作者: Joker_Sun
创建时间: 2023-05-15 21:05:52
修改时间: 2023-05-15 21:05:52 
--- 

#PVE #服务器 

---
## 什么是 PVE

	PVE全名Proxmox Virtual Environment，PVE 是一款功能强大且易于使用的开源虚拟化平台，适合用于企业和个人场景下的服务器虚拟化。
	
	Proxmox Virtual Environment（PVE）是一个基于 Debian GNU/Linux 发行版的开源虚拟化平台，提供了一套完整的虚拟化管理工具和 Web 管理界面。它支持 KVM 和 LXC 两种不同的 Linux 虚拟化技术，提供虚拟机和容器的管理和监控功能，可以方便地创建、克隆、备份和恢复虚拟机，实现高可用性和负载均衡等管理操作。

	PVE 提供了一套简单易用的全套管理工具，可以通过图形界面完成大部分虚拟化管理操作，包括创建、克隆、删除和迁移虚拟机或容器等操作。同时，还提供了完善的 API 和命令行工具，可以在不使用 Web 界面的情况下实现完整的管理和自动化操作。

	除此之外，PVE 还提供了许多高级功能，如内存和网络基于 QoS 的管理、虚拟机与容器的混合部署、集成了 ZFS 文件系统的存储池、统一的认证与授权系统等。此外，PVE 还支持多节点的集群管理模式，在集群中，虚拟机和容器可以在多个节点之间迁移和平衡负

---
## 系统安装

### 官方网站
[Proxmox](https://www.proxmox.com/en/)
### 备份数据
安装系统后系统盘的所有数据将被清除，请先保存重要的数据再执行系统安装操作。
### 下载镜像
[Proxmox镜像下载地址](https://www.proxmox.com/en/downloads/category/iso-images-pve)
直接下载最新版即可
### 制作启动盘
- [下载启动盘烧录工具](https://etcher.balena.io/)
>[!info]- 提示
> 选择 PVE 镜像文件, 然后选中 U 盘, 点击 Flash 即可
### 安装 PVE
1. 将写入了 PVE 镜像的 U 盘插入主板的 USB 接口，启动电脑，BIOS 设置 U 盘启动，这个过程也超级简单，就不再赘述了。进入之后第一个界面如下，点击 Install Proxmox VE。
![[Pasted image 20230518140206.png]]
2. 进入这个界面，右下角点击 I gree。
![[Pasted image 20230518140251.png]]
3. 选择要安装的硬盘，我这里选择了 SSD。
![[Pasted image 20230518140337.png]]
4. 在国家地方输入 China，并选择上海时区。
![[Pasted image 20230518140356.png]]
5.  设置 root 密码。
![[Pasted image 20230518140418.png]]
6. 设置网络
>[!info]- 网络相关参数
>Management Interface：会显示所有的有线网口和无线网口，这里选择 eno 开头随意一个无线网口即可
>
>HostName：主机名，DNS 域名，填写一个自己喜欢的，比如 pve. com
>
>IP Address：IP 地址，填写当前网络环境同网段下未被分配过的 IP 号即可，例如当前网络的入口地址为 10.1.2.1，那么 ip 地址可以是 10.1.2. x（x:1~254）
>
>Getway：网关，根据实际填写，例如上面的：10.1.2.1
>
>DNS Server：DNS 服务器，根据实际填写，例如上面的 10.1.2.1

![[Pasted image 20230518141742.png]]
![[Pasted image 20230518141759.png]]
7. 走到这一步，就安装成功了，这里输入 root 和 pwd 就可以登陆了。
![[Pasted image 20230518141829.png]]
---
## 浏览器访问
在浏览器输入上面设置的 IP 地址加上8006端口号访问，用户名 root，密码即上面安装的时候设置的，就可以登录了。
![[Pasted image 20230518141905.png]]
登录进来之后，页面是这样的
![[Pasted image 20230518141918.png]]

---
## 系统后处理
### PVE Tools
#### 官方地址
[官方地址](https://github.com/ivanhao/pvetools)
![[Pasted image 20230518155947.png]]
#### 删除企业源
```shell
rm /etc/apt/sources.list.d/pve-enterprise.list
```
#### 安装 PVETools
```shell
export LC_ALL=en_US.UTF-8
apt update && apt -y install git && git clone https://github.com/ivanhao/pvetools.git
cd pvetools
./pvetools.sh
```
#### 启动 PVETools
刚安装会自动启动 PVETools，如果当前重启了 PVE，需要重新进 PVETools，可通过以下命令进入
```shell
ls
```
```shell
cd pvetools
```
```shell
./pvetools.sh
```
#### 配置 apt 国内源
	更换国内源：点击更换国内源 => 选择 aliyun.com
	关闭企业更新源：点击关闭企业更新源
#### 安装配置 zfs 最大内存
	zfs 建议内存为总内存的 1/4 或 1/8 ，系统默认会占用 8G，如果当前电脑只有16G，相当于吃掉了一半的内存，建议设置为2
	安装过程中建议关闭邮件提醒
#### 安装 VIM
	安装VIM，随便选一个就行
#### 配置 PCI 硬件直通
	什么是 PCI 硬件直通：PCI 硬件直通，也称为 PCI Passthrough，通过 PCI Passthrough，您可以将物理设备直接分配给虚拟机，使虚拟机可以直接访问物理设备，从而提高虚拟机的性能和功能。
	1.开启物理机硬件直通支持（推荐）
	2.配置显卡直通（如果有显卡的话）
	3.配置qm set，可以分配虚拟机硬件
#### 配置 PVE 的 Web 界面显示传感器温度
	点击配置 PVE 的 Web 界面显示传感器温度开启功能
#### 去除订阅提示
	点击去除订阅提示
#### 设置中文
![[Pasted image 20230518160058.png]]

---
### Zerotiler 安装

---
### Wifi 设置
####查看是否配置无线网卡
```shell
ip a
```
>[!info]
> 1. <font color="#ffc000">查看是否有 wlp 的无线网卡，没有的话下面都不用看了</font>
> 2. 使用 wifi 前需要<font color="#ffc000">替换 aliyun 企业源</font>，并关闭有效订阅提示 [[PVE#PVE Tools]]

#### 激活无线网卡
 ConnMan 是什么

	ConnMan 是一个开源的网络连接管理器，它旨在提供一种简单而灵活的方式来管理网络连接。它可以管理各种类型的网络连接，包括无线、有线、蓝牙和移动宽带连接。ConnMan 还支持自动配置网络连接，包括 DHCP、IPv4、IPv6和 DNS 设置。它还提供了一组 API，供其他应用程序使用，以管理网络连接。ConnMan 最初是由 Intel 开发的，现在已经成为 Linux 的一部分，并得到了广泛的应用。

1. 安装 connman
```shell
apt-get install connman
```
2. 进入 connman 控制器
```shell
connmanctl 
```
3. 激活 wifi
```shell
connmanctl> enable wifi
```
4. 扫描 wifi
```shell
connmanctl> scan wifi 
Scan completed for wifi
```
5.  浏览 wifi
```shell
connmanctl> services 
    你家wifi名字         wifi_685d43940516_54656e64615f423231413830_managed_psk
...      ...
```
6.  开启代理
```shell
connmanctl> agent on
Agent registered
```
7. 连接 wifi
```shell
connmanctl> connect wifi_685d43940516_54656e64615f423231413830_managed_psk 
Agent RequestInput wifi_685d43940516_54656e64615f423231413830_managed_psk
Passphrase = [ Type=psk, Requirement=mandatory, Alternates=[ WPS ] ]
WPS = [ Type=wpspin, Requirement=alternate ]
Passphrase? 输入密码
Connected wifi_f8d111090ed6_6d617269636f6e5f64655f6d6965726461_managed_psk
```
8. 退出控制器
```
connmanctl> quit
```
#### 生成 wpa-psk 密钥
[[Wpasupplicant]] 

1. 安装 wpasupplicant
```shell
apt-get install -y wpasupplicant
```

2. 创建密钥
```shell
wpa_passphrase "wifi名称" "wifi密码"
```
3. 搜索 wifi
```shell
wpa_cli -i wlp2s0 scan         　     #搜索附件Wi-Fi热点
wpa_cli -i wlp2s0 scan_result 　      #显示搜索Wi-Fi热点
wpa_cli -i wlp2s0 status              #当前WPA/EAPOL/EAP通讯状态
```
#### 编辑网络配置
```shell
/etc/network/interfaces
```
处理过后
```shell
auto lo
iface lo inet loopback

#iface enp7s0 inet none

auto wlp0s20f3
iface wlp0s20f3 inet static
	address 10.1.2.222
	gateway 10.1.2.1
	wpa-conf /etc/wpa.conf #生成的配置的地址
```

---
### Nat 桥接虚拟机
