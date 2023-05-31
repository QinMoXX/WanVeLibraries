---
title: Ubuntu
author: QinMo
date: 2023-04-26 15:19:18
modified_date: 2023-05-12 16:06:45 
tags: Document Linux
alias: 
---

# 更换 apt 源
由于官方原本 apt 下载过慢，更换国内下载源能够获得更好的安装体验。

Ubuntu 的软件源配置文件是 `/etc/apt/sources.list`。将系统自带的该文件做个备份，将该文件替换为下面内容，即可使用选择的软件源镜像。

```Shell
# root权限修改配置文件
sudo vim /etc/apt/sources.list 
```

## 清华源
> [!INFO] Ubuntu 22.04 LTS
```Shell
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse

# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse

deb http://security.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://security.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
```


# 挂载 ZFS

> 参考资料:
> [zpool-import.8 — OpenZFS documentation](https://openzfs.github.io/openzfs-docs/man/8/zpool-import.8.html)
> [如何在 Ubuntu 上使用 ZFS 文件系统 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/33833767)
> [ubuntu22.04挂载zfs_bpool rpool_ubuntu22.04的博客-CSDN博客](https://blog.csdn.net/qq_45677678/article/details/126990218)
> [[Solved] "mount: unknown filesystem type 'zfs_member'" installing ZFS / Installation / Arch Linux Forums --- [已解决] “mount: unknown filesystem type 'zfs_member'” 安装 ZFS / 安装 / Arch Linux 论坛]( https://bbs.archlinux.org/viewtopic.php?id=164124 )
>[System Administration Commands (8) — OpenZFS documentation](https://openzfs.github.io/openzfs-docs/man/8/index.html) 

## 查询硬盘
```Shell
sudo fdisk -l
```

`/dev/sdb2` 是 ZFS 文件系统的池，将成为最终要挂载的文件目录。
```
```console
Disk /dev/sdb: 931.51 GiB, 1000204886016 bytes, 1953525168 sectors
Disk model: QEMU HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 8E007F6F-FE22-11ED-8229-6BEAA7F17FEE
Device Start End Sectors Size Type
/dev/sdb1 128 4194431 4194304 2G FreeBSD swap
/dev/sdb2 4194432 1953525127 1949330696 929.5G FreeBSD ZFS
```

## 安装 ZFS
对于较新的版本 Ubuntu 安装包为 zfsutils。
```Shell
sudo apt install zfsutils
```

zfsutils 提供了一系列的 zfs 管理工具。

## 导入池
查看当前池的状态
```Shell
sudo zpool status
```
如若 `no pools available` 代表当前没有接入的池。

```Shell
sudo zpool import
```

能够识别到正确的池配置：
```txt
pool: pool1
id: 1833087310936509670
state: ONLINE
status: The pool was last accessed by another system.
action: The pool can be imported using its name or numeric identifier and
the '-f' flag.
see: https://openzfs.github.io/openzfs-docs/msg/ZFS-8000-EY
config:
pool1 ONLINE
sdb2 ONLINE
```

zpool 命令导入 pool1
```Shell
sudo zpool import pool1
```

提示该池已被其他系统使用，在命令中添加 `-f` 参数
```txt
cannot import 'pool1': pool was previously in use from another system.
Last accessed by truenas.223.6.6.6 (hostid=ae177ce4) at Tue May 30 03:30:01 2023
The pool can be imported, use 'zpool import -f' to import the pool.
```

```Shell
sudo zpool import -f pool1
```

此时命令成功执行
```txt
pool: pool1
state: ONLINE
		status: Some supported and requested features are not enabled on the pool.
		The pool can still be used, but some features are unavailable.
action: Enable all features using 'zpool upgrade'. Once this is done,
		the pool may no longer be accessible by software that does not support
		the features. See zpool-features(7) for details.
config:

		NAME STATE READ WRITE CKSUM
		pool1 ONLINE 0 0 0
		sdb2 ONLINE 0 0 0
```

再查看池列表信息:
```Shell
sudo zfs list
```

设备已挂载 pool1 在 /pool1 目录下
```txt
NAME USED AVAIL REFER MOUNTPOINT
pool1 11.8M 899G 96K /pool1
pool1/.system 10.8M 899G 112K legacy
pool1/.system/configs-bd26c8fd36fd4618a75dffa52c04f828 448K 899G 448K legacy
pool1/.system/cores 96K 1024M 96K legacy
pool1/.system/rrd-bd26c8fd36fd4618a75dffa52c04f828 9.24M 899G 9.24M legacy
pool1/.system/samba4 312K 899G 312K legacy
pool1/.system/services 96K 899G 96K legacy
pool1/.system/syslog-bd26c8fd36fd4618a75dffa52c04f828 444K 899G 444K legacy
pool1/.system/webui 96K 899G 96K legacy
pool1/p1 100K 899G 100K /pool1/p1
```