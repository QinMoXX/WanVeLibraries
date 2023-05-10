### Asset和对象快照

#### 1.1 自动对象快照

##### 对象快照可以支持自动快照。在新建测试时，可以进行配置，并设置捕捉频率。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-auto-object.png)

##### 捕捉到的对象快照，可以在“报告页面 > 内存 > 对象快照”中查看，同时，也会在报告页面下方的Range Ruler中按时间顺序被标注出来。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-object-list.png)

##### 在对象快照详情页面，点击某一个Asset，包含这个Asset的对象快照小方块会被高亮显示，便于跟踪这个Asset的生命周期。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-ranger-ruler.png)

#### 1.2 对象快照详情

##### 在对象快照详情上方，列举了几项统计信息，轻触右边帮助标志可获取其该项统计的含义、推荐值、以及优化建议。包括：ShaderLab、SerializedFile、重复字体个数、超大字体个数等等。可以帮助用户快速查看和定位快照中不合理的资源.

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-shader-lab.png)

##### 点击上方的统计信息，可以快速定位相关的Asset。例如点击ShaderLab，可以快速选中Class为Shader的Asset。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-shader-details.png)

### 绑定检测报告

#### 2.1 绑定静态资源检测报告

##### 使用UPR Asset Checker进行本地资源检测，可以帮助开发者尽早发现资源文件中存在的问题。通过绑定资源检测报告，在对象快照详情页中，查看资源的Height,Width等静态信息。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-bind-asset.png)

##### 对于已绑定静态资源检测报告的对象快照，可以在“Asset > Texture2D”类中，查看到相关资源的Width、Height、MaxSize、MipMapEnabled和Format信息。对象名字旁边括号内的数字表示数量，当对象出现的次数大于1时，请检查资源是否存在冗余的情况。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-asset-info.png)

#### 2.2 绑定Shader变量检测报告

##### 使用UPR Package可以采集Shader变体数据。在绑定Shader变量检测后，可以在对象快照详情页中，查看资源的Platform,Tier等静态信息。对于已绑定静态资源检测报告的对象快照，可以在CLASSNAME = Shader时，查看到相关资源的Shader Keywords、Shader Pass Name、Shader Platform、Shader Tier和Count信息。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-shader-info.png)

### 对象快照对比

#### 3.1 同一session对象快照对比

##### 可以选择多个对象快照进行对比，差异结果按照类别分类展示。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-compare-snapshots.png)

##### 对比结果中，括号内的数字为在当前TYPE和CLASSNAME下该Object出现的次数。当同一TYPE和CLASSNAME下Object出现的MemorySize不唯一时，Total数值后标有Info标签来记录MemorySize BreakDown。例如：37.42kb × 1 + 27.31kb × 3 表示37.42kb出现1次，27.31kb出现3次。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-break-dowm.png)

#### 3.2 2个对象快照对比

##### 2个对象快照进行对比时，对比结果中可以查看到2个对象快照的对比内存增量图。悬浮右侧Info标签，可以查看到内存增量的计算方式。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-two-graph.png)

##### 通过查看矩阵图中色块的大小，可以直观地观察内存增量的大小（下方列表无排序）。同时，点击矩阵图方块，下方列表会自动过滤出相关Asset。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-two-select.png)

#### 3.3 跨Session对象快照对比

##### UPR支持跨Session对象快照对比的功能。选择不同测试中的对象快照进行对比，便于观察对象资源在内存占用上的变化。从而帮助分析各测试之间，资源使用相关的配置或代码改动所产生的问题。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-compare-btn.png)

##### 在我的项目页面，点击数据对比，可以选择区间报告对比、对象快照对比、或者内存快照对比。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-three-entry.png)

##### 在点击Session后，可以查看每个Session下的对象快照，选择后，可以进行对比。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-how-choose.png)

### 内存快照

#### 4.1 内存对象的引用Path

##### 配合UPR Desktop使用，可以获取到内存快照。查看内存分配详情，可以便于分析内存之间的调用关系。内存分配信息分为Managed Objects和Native Objects。内存分配信息均按类型分类展示。

![image-win-ui-stop](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/winupr/zh/memory-snapshot-m-sort.png)

##### Managed Object支持按Owned Size和Ref Count大小排序。

##### 点击类型名称，可以展开并获得该类型下的所有实例。

##### 点击实例名称，可以查看内存分配详情，包括字段详细信息，被引用详情和引用。

![image-win-ui-stop](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/winupr/zh/memory-snapshot-m-all.png)

##### 被引用详情包括所有从root开始的引用路径。

![image-win-ui-stop](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/winupr/memory-snapshot-root-path.png)

##### 点击被引用详情和引用表中的实例名称，可以继续访问该实例的内存分配详情。

![image-win-ui-stop](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/winupr/zh/memory-snapshot-click.png)

##### Native Objects支持按Is Persistent、Is DontDestroyOnload、Owned Size和Ref Count大小排序。

![image-win-ui-stop](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/winupr/en/memory-snapshot-n-sort.png)

##### Native Objects类型信息，点击实例名称，可以查看Flags信息和引用详情。

![image-win-ui-stop](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/winupr/zh/memory-snapshot-n-all.png)

#### 4.2 内存快照对比

##### 内存快照支持同一Session下的对比、以及跨Session的2个快照的对比。对比结果中可以查看到2个内存快照的对比内存增量图。悬浮右侧Info标签，可以查看到内存增量的计算方式。通过查看矩阵图中色块的大小，可以直观地观察内存增量的大小（下方列表无排序）。同时，点击矩阵图方块，下方列表会自动过滤出相关类型。

![image-win-ui-stop](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-memory-compare.png)

### 资源加载

#### 5.1 资源加载信息

##### 通过在项目中导入UPR Package，并且在UPR Tools中勾选所需要收集的资源，UPR Package可以帮助在游戏中收集资源加载信息。捕捉到的资源加载信息，可以在“报告页面 > 资源 > 资源加载”中查看。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-load-asset.png)

##### 选中含有资源加载信息的帧，可以在页面下方查看到资源加载的详情。Asset详情包括：Function Name、Name、Return Type和Total Time 。AssetBundle详情包括：Function Name、Path、Return Type、和Total Time 。Instantiate详情包括：Function Name、Original、Return Type、和Total Time 。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-load-details.png)

#### 5.2 Asset趋势图

##### 在“资源加载”页面，点击“Asset Trend”可以查看Asset详情。Asset趋势图展示了Asset Load/Instantiate的趋势曲线，可以帮助用户查看这个Asset的生命周期。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-asset-trend.png)

##### 如果Asset的Load或者Instantiate信息被捕捉到的话，也可以在对象快照详情页中查看这个Asset的详情。点击这个对象，可以查看到。

![image-home-page-login](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/upr-images/assetcheck/assetDoc/zh-snapshot-trend.png)