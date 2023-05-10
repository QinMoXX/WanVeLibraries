### Threads特性

-   在CPU - 线程时序页面中，点击CPU耗时图中一帧，可在下方查看到该帧的内存调用分析栈。
-   对于2017.3以上版本的项目，新建测试时，在高级设置中开启「捕捉Render线程」，并配合最新版的Windows/Mac UPR客户端进行测试，不仅可以查看到Main Thread的性能数据，还能查看Render线程。
-   FlameGraphs按时间顺序从左到右排序。水平轴代表每个堆栈的CPU时间长度，垂直轴代表处于活动状态的堆栈。

### 使用手册

##### 查看某一帧帧的内存调用分析栈。

![image-interval-ruler](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/report-page/select-frame-chart.png)

##### Threads页面主要分为Mini Map区域和Stack View区域。

![image-interval-ruler](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/report-page/navigation.png)

> Mini Map：单击并拖动以将视图缩小到特定范围，并沿任一轴滚动以平移。

![image-interval-ruler](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/report-page/mini-map.png)

> Stack View：
> 
> ‘+’ : zoom in
> 
> ‘-’ : zoom out
> 
> ‘0’ : 缩放还原
> 
> ‘r’ : 收起FlameGraphs中的递归
> 
> ‘Cmd+F/Ctrl+F’ : 搜索

##### 查看函数调用信息。

![image-interval-ruler](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/report-page/stack-tree.png)

> 点击Threads页面中的某一函数，可以在下方查看该函数的详细调用信息，以及该函数在这一帧，以及全部时间中的耗时详情。