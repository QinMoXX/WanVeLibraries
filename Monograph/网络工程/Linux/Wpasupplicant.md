---
作者: Joker_Sun
创建时间: 2023-05-23 16:16:15
修改时间: 2023-05-23 16:29:38 
--- 

#Linux #wifi

---
## 什么是 wpasupplicant
wpasupplicant 是一个用于 Linux 和 UNIX 操作系统的开源软件，它提供了一个实现 WPA 和 WPA2协议的客户端程序，用于<font color="#ffc000">连接和管理无线网络</font>。wpasupplicant 可以与多种无线网络接口设备（如 Wi-Fi 网卡）配合使用，支持多种加密方式和认证协议，包括 WEP、WPA-PSK、WPA-EAP、802.1X 等。

使用wpasupplicant连接无线网络需要在配置文件中指定无线网络的SSID、加密方式、密码等信息，然后通过wpasupplicant客户端程序与无线网络进行握手和认证，最终建立安全的无线连接。wpasupplicant还提供了一些管理命令和选项，用于查询和配置当前的无线网络连接状态，如查看连接速度、信号强度、IP地址等信息，或者重新连接、断开连接等操作。

需要注意的是，wpasupplicant 是一款高级的无线网络管理工具，使用时需要具备一定的 Linux 系统和网络知识，否则可能会导致网络连接失败或者安全漏洞。

---
## 常用的 Wpasupplican 命令
wpa_supplicant 是一个用于连接 WiFi 网络的软件工具，它包含了许多命令，以下是一些常用的命令：
### 安装 wpa_supplicant
```shell
apt-get install -y wpasupplicant
```
### 启动 wpa_supplicant 守护进程并连接到指定的 WiFi 网络。 
```shell
wpa_supplicant -B -i <interface> -c <config_file>
#例如
wpa_supplicant -B -i wlp0s20f3 -c (wpa_passphrase "1001" "4001001111")
```
### 启动 wpa_supplicant 的命令行界面，可以在命令行中执行各种操作，如扫描无线网络、连接网络等。
```shell
wpa_cli
```
###  生成一个加密的 WiFi 密码，可以将其添加到 wpa_supplicant 的配置文件中。
```shell
wpa_passphrase <SSID> <password>
```
###  指定无线网卡的驱动程序和配置文件来启动 wpa_supplicant。
```shell
wpa_supplicant -D <driver> -i <interface> -c <config_file>
```
    
###  查看 wpa_supplicant 命令的帮助信息。
```shell
wpa_supplicant -h
```
###  扫描可用的无线网络
```shell
wpa_cli scan
```
    
###  创建一个新的网络配置文件。
```shell
wpa_cli add_network
```
    
### 设置网络配置文件中的选项，如 SSID、密码等。
```shell
wpa_cli set_network <id> <variable> <value>
```
    
###  选择要连接的网络。
```shell
wpa_cli select_network <id>
```
    
### 启用网络连接。
```shell
wpa_cli enable_network <id>
```


这些命令只是 wpa_supplicant 的一部分功能，还有许多其他的命令和选项，你可以使用 `man wpa_supplicant` 命令来查看完整的帮助文档。
