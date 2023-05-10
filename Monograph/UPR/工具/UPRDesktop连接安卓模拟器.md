##  UPRDesktop版本

> 下载UPR 桌面版（windows），要求版本为`2.2.1.2`及以上。
##  使用UPR自带ADB连接模拟器

> 进入到 Windows UPR中的platform-tools文件夹中，执行如下指令

``` shell
./adb.exe connect 127.0.0.1:[端口号]
```
## 在Windows系统下，不同的模拟器需要设置不同的端口
-   [推荐]夜神安卓模拟器夜神安卓模拟器 `62001`
-   逍遥安卓模拟器逍遥安卓模拟器 `21503`
-   BlueStacks（蓝叠安卓模拟器）`5555`
-   雷电安卓模拟器雷电安卓模拟器 `5555`
-   天天安卓模拟器天天安卓模拟器 `5037`
-   网易MuMu（安卓模拟器）网易MuMu（安卓模拟器） `7555`
-   安卓模拟器大师安卓模拟器大师 `54001`
-   Genymotion `5555`

> 查看移动设备是否连接成功，在当前文件夹下执行如下指令。

```shell
./adb devices
```

> 如果出现如下信息则说明设备连接成功

```shell
List of devices attached
127.0.0.1:7555        device
```
## 使用Windows UPR进行测试，选择ADB链接下对应的模拟器
![[UPR连接模拟器.png]]
## 点击Start开始测试
<font color = 1AABDD>需要在模拟器启动游戏后在点击Start开始测试，否则不会记录</font>
![[UPR测试阶段图.png]]