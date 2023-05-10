#UPR #AssetCehcker
## 为什么要使用Asset Check
<font size = 2>用于本地资源检测，帮助开发者尽早发现资源文件中存在的问题</font>
- <font size = 2>支持所有版本的Unity项目</font>
-   <font size = 2>不依赖Unity Editor，无需安装绿色运行</font>
-   <font size = 2>检测速度极快，可在UPR中查阅检测结果</font>
-   <font size = 2>支持命令行模式，可与CI/CD工具轻松集成，实现自动化检测</font>
-   <font size = 2>规则库持续更新</font>
-   <font size = 2>支持AssetBundle冗余检测</font>
-   <font size = 2>支持静态代码分析</font>

---
## 下载地址
- [AssetChecker Win](http://10.40.1.36/Data/UPR/SoftWare/unity-asset-checker-win-latest.zip)
- [AssetChecker Mac](https://upr-1314001764.cos.ap-shanghai.myqcloud.com/asset_check/unity-asset-checker-mac-latest.zip)   
---
## 运行环境
[Python2.7.x](http://10.40.1.36/Data/UPR/SoftWare/python-2.7.1.amd64.msi)
```ad-warning
title:确认本地环境
	1. 打开命令提示符或搜索cmd
	2. 输入python查看版本
![[Python环境.png|400]]
```

```ad-failure
title:cmd输出异常
	检查：环境变量->系统变量->添加Python安装路径
```
---
## 操作手册
### 1. 下载 AssetCheck Win 程序。
### 2. 解压缩zip包。
### 3. 查看使用帮助 。
>cmd到工具目录后使用下列指令

通过`assetcheck.exe -h`可以查看到如下帮助信息
```shell
usage: assetcheck.py [-h] [--projectId PROJECTID] [--config CONFIG]
                     [--project PROJECT] [--serverIp SERVERIP]
                     [--uploadServer UPLOADSERVER]
                     [--browseServer BROWSESERVER]
                     [--includePaths INCLUDEPATHS]
                     [--excludePaths EXCLUDEPATHS]
                     [--platform {Android,iOS,PS4}] [--groupReportByAsset]
                     [--groupReportByRule] [--logFile LOGFILE]
                     [{generate-config,check,asset-bundle-redundancy-check,abcheck,code-analyze}]

asset check tool

positional arguments:
  {generate-config,check,asset-bundle-redundancy-check,abcheck,code-analyze}
                        Specify action

optional arguments:
  -h, --help            show this help message and exit
  --projectId PROJECTID
                        Specify project id
  --config CONFIG       Specify configuration file for check rules
  --project PROJECT     Specify project location
  --serverIp SERVERIP   Customized UPR server for uploading and browsing
                        results
  --uploadServer UPLOADSERVER
                        Customized UPR server address for uploading analyze
                        results
  --browseServer BROWSESERVER
                        Customized UPR server address for browsing visualized
                        results
  --includePaths INCLUDEPATHS
                        Specify include paths
  --excludePaths EXCLUDEPATHS
                        Specify exclude paths
  --platform {Android,iOS,PS4}
                        Specify build platform
  --groupReportByAsset  Group reports by asset
  --groupReportByRule   Group reports by rule, default is true
  --logFile LOGFILE     Log file
```

### 4. 资源检测

#### 4.1 生成配置文件。

`assetcheck.exe generate-config`

> 您也可以在页面上[自定义配置文件](https://upr.unity.cn/asset-check-config)，生成config.yaml，并将config文件放至asset checker文件夹内。
#### 4.2. 检测Unity工程。

`assetcheck.exe --project=<project_path> --projectId=<project_id>`

> <project_path>为Unity工程所在路径，<project_id>为UPR项目Id。
> 
> <project_id>非必需项。如果填写了<project_id>，检测结束后，可直接在UPR网站该项目内查看检测结果。

#### 4.3. 查看检测结果。

直接打开rule_report.yaml文件查看。  
或者登陆UPR网站查看。  
在检测时如果填写了<project_id>，检测结束后，可直接在UPR网站该项目内查看检测结果。  
UPR网站支持离线「上传资源检测文件」。可直接将assetcheck文件夹内的assetcheck_result.json文件上传，查看检测结果。
![[资源检测_检测导入.png]]
点击「查看」可查看资源详情
![[资源检测_查看.png]]
查看检测结果
![[资源检测_检测结果.png]]
#### 4.4 设置待检测资源的路径范围

assetcheck支持通过命令行参数 --includePaths 和 --excludePaths 来控制待检测资源所在的文件夹集合  
(注意，此参数中填入的路径分隔符在1.10.2版本之前不会被自动转换为操作系统默认)

> 例1：此时仅有Assets目录(及子目录)下的资源会进入检测，其余资源会被忽略

```
assetcheck.exe --project="C:\unity\sample201904" --includePaths="Assets"
```

> 例2：此时Assets目录下Scripts和Scenes这两个子目录中的资源会被忽略，其他Assets下的资源会进入检测

```
assetcheck.exe --project="C:\unity\sample201904" --includePaths="Assets" --excludePaths="Assets\Scripts,Assets\Scenes"
```

> 例3：--excludePaths单独也可以发挥作用

```
assetcheck.exe --project="C:\unity\sample201904"  --excludePaths="Third-party-repo,Assets\Lib"
```

（此外注意，AssetChecker默认已经将项目根目录下的Package和Library目录忽略）

AssetChecker还支持在config.yaml中对某类别的规则或者某条具体规则设置生效的路径范围，配置的生效优先级为：规则 > 类别 > 项目，高优先级的配置会直接覆盖低优先级

> 例4：当项目级别（命令行参数）和规则设置中都配置了includePaths或者excludePaths时，对于”Video size limit“这条规则，仅有它自己的路径范围设置生效，即Assets/Video下的除了CGExport外的资源将参与检测；对于所有其他规则，则适用项目级别的配置，即Assets目录下的资源都被跳过

```shell
assetcheck.exe --project="C:\unity\sample201904"  --excludePaths="Assets"
```

```yaml
(...)
- category: Video
  enabled: true
  includePaths: Assets/Included_0,Assets/Included_1
  excludePaths: Assets/Ignored_0,Assets/Ignored_1
  rules:
  - name: Video size limit
    description: Size of imported video should not over limit, default at 256MB
    includePaths: Assets/Video,Assets/VideoClip
    excludePaths: Assets/Video/CGExport
    enabled: true
    platform: All
    customParameters:
      sizeLimit: 256
```

#### 4.5指定服务器地址

对于购买了UPR企业版的用户，可以通过--serverIp参数来指定AssetCheck，AssetBundleCheck，CodeAnalyze的检测结果的上传目的服务器地址，此时使用协议默认为http，上传端口默认为8080，展示页面的端口默认为3005

```
assetcheck.exe --project="C:\unity\sample201904" --serverIp="123.123.123.123"
----
2020-08-13 15:33:07,407 INFO: Current version is 1.11.0
2020-08-13 15:33:07,408 INFO: {***}
2020-08-13 15:33:07,408 WARNING: No project id provided, report will not be uploaded to upr server
2020-08-13 15:33:07,408 INFO: Upload server set to: http://123.123.123.123:8080
2020-08-13 15:33:07,408 INFO: Browse server set to: http://123.123.123.123:3005

```

也可以通过--uploadServer和--browseServer来直接指定服务器的完整URL（此时--serverIp会被忽略）

#### 4.6 资源修复

Asset Checker中的部分规则支持直接对检测到缺陷的资源进行修复

在检测完成后，如果发现资源的缺陷是可以自动修复的，Asset Checker会生成fix_report.yaml文件，记录准备执行修复的资源和修复规则  
之后，用户需要执行`assetcheck.exe fix --project=<project_path>`来完成资源修复

现阶段用户可以手动删除fix_report.yaml中的部分条目来控制所要执行的修复范围，后续我们将提供过滤命令或UPR Web中的操作界面优化操作
### 5. 资源包冗余检测

#### 5.1 检测资源包

`assetcheck.exe abcheck --project=<assetbundle_path> --projectId=<project_id>`

> <assetbundle_path>为AssetBundle包所在绝对路径(其中不能包括中文)，<project_id>为UPR项目Id。
> 
> <project_id>非必需项。如果填写了<project_id>，检测结束后，可直接在UPR网站该项目内查看检测结果。
> 
> UPR项目创建方式可参考UPR App 使用手册，步骤2。点击ProjectId可以快速拷贝。

#### 5.2 查看整体检测结果

检测执行完成后，可点击运行日志末尾的URL跳转至UPR网站查看详细结果
![[资源包冗余检测.png]]
#### 5.3 查看Asset依赖关系链

点击上图中`相应的资源AB包`中某一个AssetBundle名字，可以在弹出页面中进一步查看此Asset到指定AssetBundle的依赖关系链
![[资源冗余检测_依赖查询.png]]
#### 5.4 本地解析结果

abcheck的执行结果会同时在本地以json文件格式保存在assetcheck所在文件夹下asse_bundle_analyze_result.json中  
示例：
<details>
<summary>展开查看</summary>
<pre><code class = "language-cpp"> {
  "AssetBundleAnalyzeResults": [           // AssetBundleAnalyzeResults中以AssetName和Bundle的组合为主键记录了所有被重复打包的asset相关信息
    {                                      // 例如'Cube'同时进入了'box'和'brick'两个bundle中，
      "AssetName": "Cube",                 // 此列表就会加入(AssetName:=Cube, Bundle:=box)和(AssetName:=Cube, Bundle:=brick)两条记录
      "AssetSize": 302,                    // 此Asset在AssetBundle中被压缩后的大小
      "AssetType": "Mesh",                 // 此Asset的类型 （AssetSize和AssetType与AssetName耦合，因此在多条记录中会重复）
      "Bundle": "box",                     //
      "RefPaths": [                        // 从Asset到Bundle的依赖链，其中的每一个节点代表了一个Asset，
        [25, 21],                          // 其数字ID对应于AssetBundleObjects中的AssetBundleObjectId
        [25, 107, 213]                     // 每一条路径中，结尾节点是用户指定进入Bundle中的Asset，其他是被自动连带进入的Asset
      ]                                    // 注意：如果不关心Asset依赖路径，只希望获取Asset在不同Bundle中的冗余情况，
    },                                     //      则仅解析AssetBundleAnalyzeResults中除去RefPaths的其他字段就可以完成；
    {                                      //      RefPaths需要和下面的AssetBundleObjects列表配合解析
      "AssetName": "Cube",
      "AssetSize": 302,
      "AssetType": "Mesh",
      "Bundle": "brick",
      "RefPaths": [
        [277, 286,303]
      ]
    },
    {
      "AssetName": "SwatchYellowAlbedo",
      "AssetSize": 183,
      "AssetType": "Texture2D",
      "Bundle": "box",
      "RefPaths": [
        [79, 80,21],
        [79, 80, 145],
        [79, 80, 126, 213],
        [79, 80, 52, 213]
      ]
    },
    {
      "AssetName": "SwatchYellowAlbedo",
      "AssetSize": 183,
      "AssetType": "Texture2D",
      "Bundle": "brick",
      "RefPaths": [
        [287, 288,308],
        [287, 288,278,303]
      ]
    },
    {
      "AssetName": "YellowSmooth",
      "AssetSize": 980,
      "AssetType": "Material",
      "Bundle": "box",
      "RefPaths": [
        [80, 21],
        [80,145],
        [80,126,213],
        [80,52,213]
      ]
    },
    {
      "AssetName": "YellowSmooth",
      "AssetSize": 980,
      "AssetType": "Material",
      "Bundle": "brick",
      "RefPaths": [
        [288,308],
        [288,278,303]
      ]
    }
  ],
  "AssetBundleObjects": [
    {
      "AssetBundleObjectId": 25,
      "AssetName": "Cube",
      "AssetSize": 302,
      "AssetType": "Mesh",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 277,
      "AssetName": "Cube",
      "AssetSize": 302,
      "AssetType": "Mesh",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 21,
      "AssetName": "Box",
      "AssetSize": 70,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 68,
      "AssetName": "Cube",
      "AssetSize": 71,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 40,
      "AssetName": "Platform",
      "AssetSize": 75,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 287,
      "AssetName": "SwatchYellowAlbedo",
      "AssetSize": 183,
      "AssetType": "Texture2D",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 79,
      "AssetName": "SwatchYellowAlbedo",
      "AssetSize": 183,
      "AssetType": "Texture2D",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 80,
      "AssetName": "YellowSmooth",
      "AssetSize": 980,
      "AssetType": "Material",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 288,
      "AssetName": "YellowSmooth",
      "AssetSize": 980,
      "AssetType": "Material",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 88,
      "AssetName": "Cube",
      "AssetSize": 71,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 133,
      "AssetName": "Platform",
      "AssetSize": 75,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 111,
      "AssetName": "Platform",
      "AssetSize": 75,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 107,
      "AssetName": "Cube",
      "AssetSize": 71,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 52,
      "AssetName": "Cube",
      "AssetSize": 71,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 145,
      "AssetName": "BoxSmall",
      "AssetSize": 87,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 204,
      "AssetName": "Platform",
      "AssetSize": 75,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 126,
      "AssetName": "Platform",
      "AssetSize": 75,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 103,
      "AssetName": "Platform",
      "AssetSize": 75,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 213,
      "AssetName": "SmashBoxes",
      "AssetSize": 29,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 129,
      "AssetName": "Cube",
      "AssetSize": 71,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 148,
      "AssetName": "Cube",
      "AssetSize": 71,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 278,
      "AssetName": "Cube",
      "AssetSize": 83,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 293,
      "AssetName": "Cube",
      "AssetSize": 83,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 291,
      "AssetName": "Cube",
      "AssetSize": 83,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 286,
      "AssetName": "Cube",
      "AssetSize": 83,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 303,
      "AssetName": "BoxPile",
      "AssetSize": 26,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 302,
      "AssetName": "Cube",
      "AssetSize": 71,
      "AssetType": "GameObject",
      "BuiltIn": false
    },
    {
      "AssetBundleObjectId": 308,
      "AssetName": "Ring",
      "AssetSize": 59,
      "AssetType": "GameObject",
      "BuiltIn": false
    }
  ],
  "Properties": {
    "BinaryVersion": "1.8.2",
    "DuplicatedAssets": "3",
    "DuplicatedBundles": "2",
    "ProcessTime": "1.232",
    "ProjectId": "default_project_id",
    "TotalAssets": "64",
    "TotalBundles": "3"
  }
}</code></pre>
</details>

### 6.代码缺陷检测

#### 6.1 检测项目地址

`assetcheck.exe code-analyze --project=<project_path> --projectId=<project_id>`

> <project_path>为Unity项目所在绝对路径(其中不能包括中文)，<project_id>为UPR项目Id。

#### 6.2 在网页中查看检测结果

### 7. Mac Warning解决方案

如果在Mac电脑上执行看到以下警告

```
OMP: Info #276: omp_set_nested routine deprecated, please use omp_set_max_active_levels instead.
```

请设置一下`KMP_WARNINGS="off"`，然后再启动assetcheck程序

```
KMP_WARNINGS="off" assetcheck -h
```

### 8.规则配置

-   可以为同一条检测规则，配置不同的目录和不同的阈值参数

```
- category: Texture
  enabled: true
  rules:
  - name: Texture over sizes
    description: Check texture size larger than a certain value, default is 512*512
      and 1024*1024
    enabled: true
    platform: All
    includePaths: Assets/AA
    customParameters:
      sizeThresholds: 128*128
  - name: Texture over sizes
    description: Check texture size larger than a certain value, default is 512*512
      and 1024*1024
    enabled: true
    platform: All
    includePaths: Assets/BB
    customParameters:
      sizeThresholds: 512*512
```

## 自定义规则

-   从UPR下载页面下载`Unity Package For UPR` 后导入项目工程
-   导入时可以只勾选以下文件

```csharp
UPRTools\Scripts\AssetChecker\AssetChecker.cs
```

-   执行命令时请传入Unity Editor可执行文件的绝对路径

```dos
assetcheck.exe --project=<project_path> --projectId=<project_id> --editorPath=<unity_path>
```

-   自定义规则实现参考以下文件

```csharp
UPRTools\Scripts\AssetChecker\TextureRuntimeMemoryCheck.cs
```

```csharp
using System.Collections.Generic;
using System.IO;
using UnityEditor;
using UnityEngine;

public class TextureRuntimeMemoryCheckRule : AssetChecker.BaseRule
{
    public override string Name => "Texture Runtime Memory";
    public override string Description => "Check Runtime Memory of Texture with .tga format";

    public override void Check(out List<AssetRuntimeCheck.RuleResult> results)
    {
        results = new List<AssetRuntimeCheck.RuleResult>();
        var files = Directory.GetFiles("Assets", "*.tga", SearchOption.AllDirectories);
        for (int index = 0; index < files.Length; index++)
        {
            var fileName = files[index];
            Texture tex = AssetDatabase.LoadAssetAtPath<Texture>(fileName);
            long runtimeSize = UnityEngine.Profiling.Profiler.GetRuntimeMemorySizeLong(tex);
            results.Add(new AssetRuntimeCheck.RuleResult{
                AssetPath = fileName, Properties = new Dictionary<string, string>
                {
                    {"RuntimeMemory", EditorUtility.FormatBytes(runtimeSize)}
                }
            });
        }
    }
}
```

-   样例报告 [https://upr.unity.cn/sample/asset-check/633dc81e-4b6e-466b-8cc2-b85c2b0a4fcd](https://upr.unity.cn/sample/asset-check/633dc81e-4b6e-466b-8cc2-b85c2b0a4fcd)
-   使用过程中如果有任何问题，付费用户请在微信群中提问，试用用户请在QQ群`1029672930`中提问

## 附录

#### 默认规则文件

<details>
<summary>点击展开</summary>
<pre><code class="language-cpp">- category: EditorSetting
  enabled: true
  rules:
  - name: Editor AndroidManagedStrippingLevel Setting
    description: Android设置中的ManagedStrippingLevel选项不应为Low或者Disabled
    enabled: true
    platform: Android
  - name: Editor Physics2D AutoSyncTransforms Setting
    description: 在Physics2D设置中应关闭AutoSyncTransforms
    enabled: true
    platform: All
  - name: Editor Physics AutoSyncTransforms Setting
    description: 在Physics设置中应关闭AutoSyncTransforms
    enabled: true
    platform: All
  - name: Editor BakeCollisionMeshes Setting
    description: 应设置开启BakeCollisionMeshes
    enabled: true
    platform: All
  - name: CompanyName should be set
    description: 应设置CompanyName
    enabled: true
    platform: All
  - name: Editor GraphicsJobs Setting
    description: 应设置开启GraphicsJobs
    enabled: true
    platform: All
  - name: Editor AccelerometerFrequency iOS Setting
    description: iOS设置中的AccelerometerFrequency选项应为 Disabled
    enabled: true
    platform: iOS
  - name: Editor Architecture iOS Setting
    description: iOS设置中的Architecture选项不应为Universal
    enabled: true
    platform: iOS
  - name: Editor GraphicsAPIs iOS Setting
    description: 在 iOS 的 GraphicsAPIs 设置里应只包含 Metal
    enabled: true
    platform: iOS
  - name: Editor iOSManagedStrippingLevel Setting
    description: iOS设置中的ManagedStrippingLevel选项不应为Low
    enabled: true
    platform: iOS
  - name: Build Target Icons should be set
    description: 应设置Build Target Icons
    enabled: true
    platform: All
  - name: Editor Physics2D LayerCollisionMatrix Setting
    description: 在Physics2D设置中LayerCollisionMatrix中的格子不应该都勾选上
    enabled: true
    platform: All
  - name: Editor Physics LayerCollisionMatrix Setting
    description: 在Physics设置中LayerCollisionMatrix中的格子不应该都勾选上
    enabled: true
    platform: All
  - name: Project Use Resources Folder
    description: 不建议使用Resources系统来管理asset
    enabled: true
    platform: All
  - name: Editor Graphics StandardShaderQuality Setting
    description: StandardShaderQuality选项在所有Graphics Tier中应相同
    enabled: true
    platform: All
  - name: Editor StripEngineCode Setting
    description: 应设置开启StripEngineCode
    enabled: true
    platform: All
- category: Animation
  enabled: true
  rules:
  - name: Animation Precision
    description: 动画曲线精度应小于5
    enabled: true
    platform: All
  - name: Animation Scale
    description: 动画不应具有缩放曲线
    enabled: true
    platform: All
- category: Audio
  enabled: true
  rules:
  - name: Audio Android Compression Format
    description: 检查安卓平台的音频压缩格式
    enabled: true
    platform: Android
  - name: Audio load type
    description: 检查音频加载类型
    enabled: true
    platform: All
  - name: Force to Mono
    description: 应为音频资源启用forceMono
    enabled: true
    platform: All
  - name: Audio iOS compression format
    description: 检查iOS平台的音频压缩格式
    enabled: true
    platform: iOS
- category: FBX
  enabled: true
  rules:
  - name: Animation Compression Optimal
    description: 动画资源应该使用最佳压缩方式
    enabled: true
    platform: All
  - name: FBX Read&Write Flag
    description: 应禁用FBX资源的读/写标志
    enabled: true
    platform: All
  - name: Mesh OptimizeMesh
    description: 应为网格资源启用OptimizeMesh
    enabled: true
    platform: All
- category: Mesh
  enabled: true
  rules:
  - name: Mesh Read&Write Flag
    description: 应为网格资源禁用读/写标志
    enabled: true
    platform: All
- category: Prefab
  enabled: true
  rules:
  - name: Prefab Max Particle Limit
    description: 渲染Mesh的粒子系统不宜设置过高的粒子总数
    enabled: true
    platform: All
  - name: Prefab mesh emit rate
    description: 渲染Mesh的粒子系统不宜设置过高的粒子发射速率
    enabled: true
    platform: All
  - name: Prefab mesh Read/Write flag
    description: 被预制件关联的网格资源应该关闭读/写标记
    enabled: true
    platform: All
  - name: Prefab skinned mesh render
    description: 启用Skinned Motion Vectors将以消耗双倍内存为代价提高蒙皮网格的精度
    enabled: true
    platform: All
- category: Scene
  enabled: true
  rules:
  - name: Scene Animator AlwaysAnimate
    description: 场景中包含cullingMode为AlwaysAnimate的Animator组件
    enabled: true
    platform: All
  - name: Scene Animator ApplyRootMotion
    description: 场景中包含勾选了applyRootMotion选项的Animator组件
    enabled: true
    platform: All
  - name: Scene Canvas Component
    description: 包含太多组件的Canvas可能会影响UI刷新的性能
    enabled: true
    platform: All
  - name: Scene Mesh Collider
    description: 场景不应包含mesh collider
    enabled: true
    platform: All
  - name: Scene Multiple Audio Listeners
    description: 一个场景不应包含多个Audio Listener
    enabled: true
    platform: All
  - name: Scene Render Setting
    description: 在移动平台，应在设置里关闭对雾的渲染
    enabled: true
    platform: All
  - name: Scene Rigidbody
    description: 场景中的静态GameObject不应关联Rigidbody
    enabled: true
    platform: All
  - name: Scene Undefined Tag
    description: 场景中的所有GameObject都应当添加tag
    enabled: true
    platform: All
- category: Script
  enabled: true
  rules:
  - name: Empty Update Method
    description: MonoBehavior脚本不应具有空的Update方法
    enabled: true
    platform: All
  - name: OnGUI
    description: 由于内存使用率高，不应使用OnGUI方法
    enabled: true
    platform: All
- category: Shader
  enabled: true
  rules:
  - name: Shader Texture Count
    description: Shader中的纹理个数应小于3
    enabled: true
    platform: All
- category: Texture
  enabled: true
  rules:
  - name: Texture Android compression format
    description: 检查Android平台的纹理压缩格式
    enabled: true
    platform: Android
  - name: Aniso level is not 1
    description: 纹理资源的Aniso级别不应大于1
    enabled: true
    platform: All
  - name: Texture Read&Write Flag
    description: 应禁用纹理资源的读/写标志
    enabled: true
    platform: All
  - name: Duplicate textures
    description: 检查重复纹理
    enabled: true
    platform: All
  - name: Texture edge transparent
    description: 边缘有较大透明部分的纹理应裁剪以节省内存
    enabled: true
    platform: All
  - name: Filter mode
    description: 纹理资源的过滤模式不建议为Trilinear
    enabled: true
    platform: All
  - name: Texture iOS compression format
    description: 检查iOS平台的纹理压缩格式
    enabled: true
    platform: iOS
  - name: Mipmap Flag
    description: 未压缩的纹理资源应禁用Mipmap标志
    enabled: true
    platform: All
  - name: Texture over size
    description: 检查纹理大小是否大于某个值，默认值为 512*512
    enabled: true
    platform: All
    customParameters:
      widthThreshold: 512
      heightThreshold: 512
  - name: Texture pure color
    description: 纯色纹理会浪费内存
    enabled: true
    platform: All
  - name: Texture size NPOT
    description: 纹理资源的大小应该是2的幂次
    enabled: true
    platform: All
  - name: Sprite fill rate
    description: 检查纹理资源的sprite填充率，默认阈值为0.5，并且只检查sprite类型
    enabled: true
    platform: All
    customParameters:
      fillRateThreshold: 0.5
      onlyCheckSprite: true
  - name: Texture useless alpha channel
    description: 如果纹理包含空的alpha通道，则应禁用'Alpha源'标志
    enabled: true
    platform: All
  - name: Texture wrap mode
    description: Repeat Wrap模式可能会导致纹理上出现意外的边缘
    enabled: true
    platform: All
- category: Video
  enabled: true
  rules:
  - name: Video size limit
    description: 导入的视频文件大小应小于某一限制，默认为256MB
    enabled: true
    platform: All
    customParameters:
      sizeLimit: 256
</code></pre> </details>
