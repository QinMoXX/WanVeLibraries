---
作者: Joker_Sun QinMo
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

> 参考文档：
> [PVE是否直接安装docker？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/384631972/answer/2472979826)
> [PVE系列教程(一)、PVE7.1.2版本系统安装 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/510216630)


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
[[ZeroTier]]

---
### Wifi 设置
1. 用到了 wireless tools Wifi工具，首先安装它。
```Shell
apt install iw
```

2. 使用该命令查询可用网卡。
```Shell
iw dev
```
结果中包含名为 wlp0s20f3 的无线接口。
```Shell
root@pve:~# iw dev
...
        Interface wlp0s20f3
                ifindex 3
                wdev 0x1
....
```

#### 查看网卡状态
使用该命令指定接口，获取接口状态，尖括号信息中未包含 UP 字样表示接口未激活。
```Shell
ip link show wlp0s20f3
```
在搜索可用 wifi 前需要将接口激活，指定好对应的接口名。
```Shell
ip link set wlp0s20f3 up
```

#### 搜寻可用 wifi
```Shell
iw wlp0s20f3 scan
```
或者使用 `grep` 语句匹配字符串
```Shell
iw wlan0 scan | grep [string]
```

#### 连接 wifi

连接 wifi 需要使用到工具 wpasupplicant
1. 安装 wpasupplicant
```shell
apt-get install -y wpasupplicant
```

2.  输入密码连接指定 wifi 
```Shell
wpa_supplicant -B -i wlp0s20f3 -c<(wpa_passphrase "SSID" "密码")
```
连接成功后会有如下输出
```
Successfully initialized wpa_supplicant
```

3. 配置 wifi ip 地址
```Shell
ifconfig wlp0s20f3 192.168.199.200
```
使用 `ifconfig` 命令查询接口状态，正确分配了指定IP地址
```Shell
wlp0s20f3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.199.200  netmask 255.255.255.0  broadcast 0.0.0.0
        inet6 fe80::2216:b9ff:fec0:92c  prefixlen 64  scopeid 0x20<link>
        ether 20:16:b9:c0:09:2c  txqueuelen 1000  (Ethernet)
        RX packets 95556  bytes 48739479 (46.4 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 128460  bytes 46266440 (44.1 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

#### wifi 自启动
以上的 wifi 连接方式需要手动输入，而我们想要其在系统启动时自动连接 wifi 就必须进行一些配置。
1. 修改接口文件：
```shell
vim /etc/network/interfaces
```
2. 将 wlp0s20f3 修改如下：
使用 static 配置 ip 地址，并且端口启动时执行 /root/ip.config.sh 脚本
```shell
auto lo
iface lo inet loopback

iface enp7s0 inet manual

auto vmbr0
iface vmbr0 inet manual
	bridge-ports none
	bridge-stp off
	bridge-fd 0
#	address 192.168.199.124/24	gateway 192.168.199.1
# 即将启用 wifi 作为主端口，关闭桥接功能避免冲突。

auto wlp0s20f3 # 接口开机启动
iface wlp0s20f3 inet static
	address 192.168.199.200/24
	gateway 192.168.199.1
	post-up bash /root/ip.config.sh # 启动时执行bash脚本
```
3. 创建启动脚本
```Shell
touch /root/ip.config.sh
```
4. 编辑脚本
```Shell
vim /root/ip.config.sh
```

修改例子如下：
```
#!/usr/bin/env bash
 wpa_supplicant -B -i wlp0s20f3 -c<(wpa_passphrase "1001" "4001001111")
```

重启服务器，wifi 能够自启动并且 ping 通外网地址表示成功。

---
### Nat 转发内部端口

为虚拟机创建一个虚拟接口管理内部网络是一个较好的选择，独立网段便于管理，隔离内外网优点就不一一例举了。

![[Drawing 2023-05-24 21.21.11.excalidraw]]

#### 配置 VM1 虚拟端口

1. 编辑文件
```Shell
vim /etc/network/interfaces
```

新增 vmbr1 端口，注意的是，网桥是关闭的。
```
auto vmbr1
iface vmbr1 inet static
	address 192.168.1.1/24
	bridge-ports none
	bridge-stp off
	bridge-fd 0
  post-up echo 1 > /proc/sys/net/ipv4/ip_forward
```

配置 NAT ，添加一条自启动脚本
```Shell
  post-up bash /root/iptables.config.sh
```

编辑启动脚本，使用 iptables 命令添加 NAT 条目
```Shell
#!/usr/bin/env bash
# 出站NAT转发来自'192.168.1.0/24'的端口到wlp0s20f3端口
iptables -t nat -A POSTROUTING -s '192.168.1.0/24' -o wlp0s20f3 -j MASQUERADE

# 入站NAT转发来自wlp0s20f3 445端口到 '192.168.1.100:445'
iptables -t nat -A PREROUTING -i wlp0s20f3 -p tcp --dport 445 -j DNAT --to-destination 192.168.1.100:445
# 入站NAT转发来自wlp0s20f3 433端口到 '192.168.1.100:443'
iptables -t nat -A PREROUTING -i wlp0s20f3 -p tcp --dport 443 -j DNAT --to-destination 192.168.1.100:443
```

以上是几个示例，根据实际情况自行添加。

# 存储设计

我们以 PVE 作为硬件的管理平台，存储空间的规划也是一个设计重点。
在设计上我们理应追求几个要点：
1. 高效：存储作为首要需求，外部读取写入访问过程尽量简捷；
2. 安全：多用户多服务带来了一定的权限管理需求，毕竟所有用户和服务都拥有完全的系统控制权限后患无穷；
3. 共享：不同的虚拟机中共享存储，而不是使用其他虚拟机的服务，能够单服务独立可用；
4. 可持续：服务出现问题，设备宕机都在可承受范围内，但是数据不能够丢失，即使硬件损坏将数据迁移仍然能够保证服务继续运行这是最关键的；

如何将平台利用平台的硬件设备高效的将存储服务向外部提供，这一部分在 TrueNas [[NAS]] 中展示。TrueNAS 系统为我们提供了规范细致的权限组划分，以及数据集的权限分配为多用户多服务提供有利保障。
但在 NAS 服务拥有存储管理权限的时候，我们仍然希望利用存储中的数据对外提供多媒体访问、代码仓库等其他服务，也就是说某一块或多块存储空间能够被多个虚拟机访问。

早些的时候，我们是将硬盘全交由内部的 TrueNAS 管理，其他虚拟机的访问通过 TrueNAS 提供的网络服务访问，✅这样配置简单管理方便，TrueNAS 可以直接控制访问权限。❗但随后衍生出一个问题，当 NAS 服务宕机时其他的上层服务将不可避免的陷入瘫痪。
![[Drawing 2023-05-30 14.17.24.excalidraw]]

---

⛔现在我们根据这个方案进行改进，硬件的控制权交由 PVE，利用 ZFS 文件系统虚拟机的之间访问相同的数据池，为了安全要点会将不同硬盘根据存储内容分成多个池挂载到不同的虚拟机。
![[Drawing 2023-05-30 13.39.27.excalidraw]]

> [!warning] 此方案暂时不可用
> 在我们初步尝试挂载相同 zfs 时，该方案由于数据读写不同步问题已陷入停滞。在此详细描述下问题表现：
> - 当使用 SMB 服务连接存储时，读写数据一切正常
> - 在 TrueNAS 中修改文件系统中数据，读写正常
> - 当在 Ubuntu 虚拟机中挂载硬盘，读取正常，执行写入操作其他设备无法观察到数据变更，重新挂载池修改数据丢失

不断尝试可行的操作解决这一问题，查询资料过程中总结了以下切入点：
- ZFS 权限管理
- PVE 存储类型的 Shared （我们意识到磁盘以 Directory 加入 PVE，Directory 是不共享的）
- 文件系统不支持多设备挂载的操作


---

## 硬盘分区


## 挂载硬盘
> 参考资料
> [How to: Passthrough HDD/SSD/Physical disks to VM on Proxmox VE(PVE) > Blog-D without Nonsense --- 如何：将 HDD/SSD/物理磁盘直通到 Proxmox VE 上的 VM（PVE）> Blog-D without Nonsense (dannyda.com)](https://dannyda.com/2020/08/26/how-to-passthrough-hdd-ssd-physical-disks-to-vm-on-proxmox-vepve/)
> [Passthrough Physical Disk to Virtual Machine (VM) - Proxmox VE --- 直通物理磁盘到虚拟机 (VM) - Proxmox VE]( https://pve.proxmox.com/wiki/Passthrough_Physical_Disk_to_Virtual_Machine_ (VM))

进入 PVE 的 SSH，查看当前磁盘

```Shell
ls /dev/disk/by-id
```

目录下显示的是所有硬盘的序列号，这里需要挂载的是 ata-WDC_WD10SPZX-22Z10T1_WD-WXA1A5866E8A，以 scsi 的形式挂载到虚拟机 100 上。
```Shell
qm set 100 -scsi2 /dev/disk/by-id/ata-WDC_WD10SPZX-22Z10T1_WD-WXA1A5866E8A
```

输出 Update 表示成功。
```
update VM 100: -scsi2 /dev/disk/by-id/ata-xxxxxxxxx-xxxxx_xxx
```

相关命令
移除虚拟磁盘
```Shell
qm unlink 100 --idlist scsi2
```

如果我们想附加另一个磁盘，我们可以使用-scsi1 或-scsi2 或-scsi3 等......