#UPR #UPRDesktop
## 为什么要使用UPR Desktop
	UPR桌面端解决方案，减轻测试设备性能压力，使测试过程更加顺畅。提供CLI用于自动化测试系统对接。
---
## 下载地址
[UPRDesktop](https://upr.unity.cn/download)

---
## [演示教程](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/video/win-video.mp4)
---
## 注意事项
-   UPR Windows/UPR Mac 可以配合 [[UPR Package]] 使用，获取游戏截图、收集手机内存信息，以及Lua性能分析等基本数据。
-   为解决不同网络环境下设备之间的测试，和避免网络不稳定的数据传输问题等问题，推荐使用ADB进行连接。在ADB模式下，可以截取RenderDoc。
-   新增RenderDoc功能，便于GPU的分析与性能优化。
-   新增内存快照（Memory Snapshot）功能，便于分析内存之间的调用关系，查看内存分配详情（目前仅支持18.3及以上版本）。同时，新增内存碎片功能。
-   新增Overdraw功能，帮助解决GPU端像素填充率和计算量的瓶颈问题。
---
## 操作手册
### 打包设置 
   >Unity工程在Build Settings里面勾选过<font color = 1AABDD>「Developmeng Build」「Autoconnect Profiler」</font>选项勾选后直接打包或打Android工程后打APK包
### 创建测试项目并导入UPRDesktop
>打好包后需要在[[组织项目管理#^e855b5]]中创建好测试记录，将测试记录中的Session Id拷贝到UPRDesktop
![[Pasted image 20230507220854.png]]
### 使用ADB连接到模拟器
>参考[[UPRDesktop连接安卓模拟器]]连接至安卓模拟器后进行测试
![[Pasted image 20230507222041.png]]
### UPRDesktop设置
![[Pasted image 20230507222257.png]]
>以下为参数解释说明
>同时，点击右上方设置，可以查看Logs和Logcat。
```Log
Use KCP
用于kcp数据传输（企业版专供）

Message Count Threshold
当网络传输速率差时，UPR Windows会将超过缓存阈值的记录写入文件。
当正常的记录发送完毕后，UPR Windows将从文件中读取记录，并继续发送。
文件缓存阈值默认设置为5000。

Dump Path
用于指定缓存存放位置，避免默认位置硬盘空间不足
也可以通过更改文件夹内的UPRDesktop.exe.config文件内的TempPath的setting进行更改
通过UI更改的的优先级大于UPRDesktop.exe.config文件内的TempPath的setting

Render Doc Default
设置是否默认开启Render Doc。默认是不开启。

Specify ADB Path
用于指定测试使用的adb路径，避免同其他应用产生冲突
```
### 开始测试
>点击「Start Test」, 连接UPR服务器，并开始测试。
![[Pasted image 20230507222335.png]]
### UPRDesktop工具相关功能
>点击「Add Tag」记录标签。
![[Pasted image 20230507222458.png]]

>点击「Capture Objects」记录对象快照。
![[Pasted image 20230507222552.png]]

>点击「Capture Memory」记录内存快照。
![[Pasted image 20230507222611.png]]

>测试游戏，待测试结束，点击「Stop」按钮。
![[Pasted image 20230507222644.png]]

