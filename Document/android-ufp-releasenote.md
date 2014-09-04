
# ANDROID SDK Release Note



<8月迭代>
##### 版本: v7.3.0.20140903
##### 发布日期: 2014.09.03
##### 更新：

1.合并电商墙和应用墙使用一个Activity

```
<activity
    android:name="com.taobao.newxp.view.handler.umwall.AlimamaWall"
    android:configChanges="keyboard|orientation"
    android:hardwareAccelerated="true"
    android:launchMode="singleTask"
    android:screenOrientation="portrait"/>
```
2.H5页面性能优化，首屏速度提升40%

3.Mraid协议扩展，新增支持打电话，发短信，发邮件等功能

4.优化支持唤醒淘宝客户端

5.优化一些UI，修复一些bug

##### 版本: v7.2.0.20140714
##### 发布日期: 2014.07.14
##### 更新：

1.优化推广墙样式，使用native解决方案。

2.新增内嵌H5墙。

3.整合H5Banner，H5 插屏。

4.优化包结构，去除Umeng 字段。


##### 版本: v6.7.20140604(LiteSDK)
##### 发布日期: 2014.06.04
##### 更新：

1.添加对外Apv接口

* exchagneViewMangager.reportApv

##### 版本: v6.9.5.20140523
##### 发布日期: 2014.05.23
##### 更新：

1.优化WebView,添加底部工具栏

2.新增接口用于关闭推广打开的视窗
* ExchangeViewManager.destroy()

3.添加SDK渠道设置字段
* ExchangeConstants.SDK_CHANNEL

4.更新新版utdid，修复“android.permission.WRITE_SETTINGS”权限问题

5.修复一些bug

6.增加性能优化feature，启用spdy协议代理电商墙图片请求，下载速率提升 20%，增加http-dns-1.2.0-20140308.034153.jar和spdnSDK-1.0.1-20140422.064108.jar两个jar包


##### 版本: v6.9.20140523
##### 发布日期: 2014.05.23
##### 更新：

1.优化WebView,添加底部工具栏

2.新增接口用于关闭推广打开的视窗
* ExchangeViewManager.destroy()

3.添加SDK渠道设置字段
* ExchangeConstants.SDK_CHANNEL

4.更新新版utdid，修复“android.permission.WRITE_SETTINGS”权限问题

5.修复一些bug

##### 版本: v6.7.140428(LiteSDK)
##### 发布日期: 2014.04.28
##### 更新：

1.优化WebView,添加底部工具栏

2.新增接口关闭webview
* ExchangeViewManager.disBrowser()

3.添加SDK渠道设置字段
* ExchangeConstants.SDK_CHANNEL

4.更新新版utdid

5.精简SDK，删除冗余资源文件


##### 版本: v6.9.20140402
##### 发布日期: 2014.04.02 
##### 更新：

1.添加主客户端唤醒功能

* 设置MTop相关参数后，可唤醒淘宝主客户端详情页，收藏夹，用户登录，购买等后续操作
 

2.修复一些bug

##### 版本: v6.8.20140212(芒果)
##### 发布日期: 2014.02.12 
##### 更新：

1.添加数据加载完成外部接口

2.全屏广告添加点击回调

##### 版本: v6.7.20140109（meitu）
##### 发布日期: 2013.01.09  
##### 更新：

1.修复电商墙错误页面点击“重试”后一直处于"Loading"状态

2.添加设置应用墙默认tab名称（应用墙当且仅有一个tab的情况下，顶部Actionbar标题将显示tab名称且隐藏tab）

* ExchangeConstants.DEFAULT_HANDLE_APP_WALL_TITLE


##### 版本: v6.7.20140108（meitu）
##### 发布日期: 2013.01.08  
##### 更新：

1.修复下载过程中使用‘multitask panel’关闭应用导致下载通知栏残留

2.设置默认tab为App


##### 版本: v6.8.20131230
##### 发布日期: 2013.12.30  
##### 更新：
1.信息流添加团购，应用推荐样式

2.文字链添加分段链接样式

3.全屏弹窗添加电商样式模板

4.修复一些bug

##### 版本: v6.7.20131230（meitu 定制：添加入口展示回调）
##### 发布日期: 2013.12.30  
##### 更新：
1.添加接口onHandlerVisListener 预加载入口展示回调

* exMgr.addView(ExchangeConstants.type_list_curtain, lazyHandleVp,mHandleVisListener);

2.添加UMHandleReletiveLayout

* 可扩展clicklistener


##### 版本: v6.7.20131225
##### 发布日期: 2013.12.25  
##### 更新：
1.修该referer信息

2.修复电商墙页面不可点击

##### 版本: v6.7.20131214（6.7Lite）
##### 发布日期: 2013.12.14  
##### 更新：
1.6.7 Wap 入口

##### 版本: v6.7.20131213
##### 发布日期: 2013.12.13  
##### 更新：
1.修复组件化依赖库问题


##### 版本: v6.7.20131205
##### 发布日期: 2013.12.05  
##### 更新：
1.应用墙添加轮播图功能.

* 集成自定义入口SDK时必须添加轮播大图SDK.
* 在广告位中添加大图样式的广告.

2.应用墙和电商墙添加taobao定位服务.

3.自定义入口图片支持GIF

4.全新改版的电商墙.

* 添加浏览历史，猜你喜欢，积分宝等功能.

5.在Manager中添加模块校验功能.

6.原始sdk包添加xp-demo,xp-doc

7.修复一些bug.



##### 版本: v6.6.20131129(爱奇艺精简版)
##### 发布日期: 2013.11.29  
##### 更新：
1.删除小把手h5样式的其他代码

##### 版本: v6.6.20131125
##### 发布日期: 2013.11.25  
##### 更新：
1.添加团购墙

2.更新UI

3.修复一些bug

##### 版本: v6.5.20131110
##### 发布日期: 2013.11.10  
##### 更新：
1.电商墙内部分离

2.添加反作弊信息

3.电商墙添加收藏夹功能

4.修复一些bug


##### 版本: v6.4.20131105
##### 发布日期: 2013.11.05  
##### 更新：
1.增加信息流样式

2.优化积分墙样式加载图片的大小

4.修复电商详情页无法跳转淘宝主客户端问题

##### 版本: v6.3.20131023
##### 发布日期: 2013.10.23  
##### 更新：
1.增加积分墙样式
 
2.修复一些bug

##### 版本: v6.2.20130929
##### 发布日期: 2013.09.29  
##### 更新：
1. 应用墙升级，支持多tab
* 集成方式需要在Manifest文件中注册Activity

```
  <activity
            android:name="com.umeng.newxp.view.handler.UMWall"
            android:configChanges="keyboard|orientation"
            android:screenOrientation="portrait"
            android:theme="@style/StyledIndicators" />
```
2. 新增详细商品详情页
* 集成方式需要在Manifest文件中注册Activity

```
  <activity
            android:name="com.umeng.newxp.view.UMDetail"
            android:configChanges="keyboard|orientation"
            android:launchMode="standard" />
```

3. 自定义入口支持动态文字

4. 修复一些bug

##### 版本: v6.1.20130808
##### 发布日期: 2013.08.08  
##### 更新：
1. 电商墙添加“改改”样式
2. 重新设计了wap样式
3. 修复电商墙bug

##### 版本: v6.0.20130630 
##### 发布日期: 2013.06.30  
##### 更新：
1. 小把手样式添加电商墙模板
* 集成方式需要注册Activity

```
<activity
    android:name="com.umeng.newxp.view.handler.ewall.EWall"
    android:configChanges="keyboard|orientation"
    android:screenOrientation="portrait"
    android:theme="@style/StyledIndicators" />
```
* 指定UFP广告位才会出电商墙
* 电商墙搜索功能

### 应用联盟 UFP 共同迭代release note

##### 版本: v5.7.20130428  
##### 发布日期: 2013.04.28  
##### 更新：
1. 分离各个样式的jar和res文件。
2. 改进PreloadData，使PreloadData中数据可以复用。
3. List样式添加Header扩展类，可以设置头图广告，头图可以动态关闭。
4. Promoter添加filter 可以控制单个广告的过滤

##### 版本: v5.6.20130118  
##### 发布日期: 2013.01.18  
##### 更新：
1. ResUtils.clearCache 在线程中进行,避免ANR
2. ResUtil 绑定图片是加入View复用检查，避免View复用机制和异步加载有冲突。
3. 交换网络支持一个App 多个appkey
4. 添加Feature Promoter 数据层
* 添加接口发送展示报告
* 添加接口调用打开方式
5. Report 添加字段JSON_KEY_CARRIER(运营商)
6. 支持样式 PushNative
7. 添加字段mc idmd5
8. 图片资源移至通用目录drawable下


##### 版本: v5.5.20121219  
##### 发布日期: 2012.12.19  
##### 更新：
1. 移除Wap，内嵌，文字链等样式，只保留自定义入口样式。

##### 版本: v5.5.20121023  
##### 发布日期: 2012.10.23  
##### 更新：
1. 修复asynctask同时使用过多导致crash
2. 修复缓存图片损坏导致无法加载图片
3. 修复图片圆角化过程中可能导致的OOM
4. 圆角图标展示可ExchangeConstants.ROUND_ICON 控制
5. 修复filter report layout_type 参数错误
6. 添加下载中止事件report(完成，失败)
7. 小把手，Banner，文字链，push,内嵌支持preload
8. 添加字段
   （下载report[dsize=已下载多少 dtime=下载中断时间 dpcent=下载完成百分比 ptimes=下载重复次数 ts=下载完成时间戳] report[access=当前网络 access_type=运营商]）
9. 添加小把手展示 关闭监听器
10. android 4.0以上设备支持通知栏下载过程控制
11. 悬浮窗样式支持文字消息Dialog形式。
12. Banner样式移出

##### 版本: v5.3.20120822  
##### 发布日期: 2012.08.22  
##### 更新：
1. 小把手添加全图标样式。
2. 添加接口ExchangeDataService.setTemplate 可设置List显示模板 0为applist 1为全Icon样式
3. Banner开放展示回调接口，添加方式 ExchangeDataService.initializeListener = yourlistenter
       

##### 版本: v5.2.20120803
##### 发布日期: 2012.08.03
##### 更新：
1. 轮播大图  ItemView使用xml布局方便配置 添加图片绑定过程接口（加载图片时可显示进度条）（UFP）
2. 内嵌样式，小把手ListView 模板 ListView 第一页如果不足将先先请求下一页填充再判定footerview功能
3. 拨打电话首先弹出确认框再拨打(UFP)
4. 如果系统没浏览器，浏览器打开将提示用户。
5. 自定义入口和内嵌入口增加新广告提示功能（UFP）
6. 修复Banner 只有一条广告是不发送展示报告。


##### 版本: v5.2.20120716
##### 发布日期: 2012.07.16
##### 更新：
1. 调整Banner样式默认布局 放大图标和文字区域
2. UFP增加推送弹窗样式
3. 修复在应用退出时进程关闭，下次进入应用再次点击更新情况下， 重复下载问题。 
4. 去掉不需要的umeng_xp_download_notification.xml布局文件
5. 调整下载完成report 发送机制，改为在Service中发送
6. 调整Wap样式

##### 版本: v5.1.20120615
##### 发布日期: 2012.06.15
##### 更新：
1. 通过android组件形式提供下载。下载之后一个SDK包含多个友盟服务。
1. 修复存在两个不同步的style文件，导致在英文操作系统下载详情布局问题。
1. 修复部分Listener 被混淆问题。
1. 修复下载框图标重叠问题。
