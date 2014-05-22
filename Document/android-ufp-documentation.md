## **<center>Android UFP SDK使用指南</center>**

### 1. 简介： 
* 服务简介:
UFP提供了App自主推广，广告管理，App间交叉推广等功能。
* SDK 简介：
  * 提供 _**<font color='green'>8</font>**_ 种样式:  横幅(Banner), 列表(TableView), 文字链(TextLink), Wap(Webview), 小把手(Handle), 头图(HeadlineView), 弹框(Dialog), Icon列表(GridView)

===


### 2. 建立广告位，获取Slot id

* **登录** http://ufp.umeng.com 

    <div style="float:left;width:100%;"><img src="http://dev.umeng.com/images/ios/login.png"/></a></div><div style="clear:both"></div>

* **登录后** 将看到您现有的广告位信息，如：
    <div style="float:left;width:100%;"><img src="http://dev.umeng.com/images/ios/slots.png"/></a></div><div style="clear:both"></div>

* **新建广告位**，点击“添加广告位”, 选择新建的广告位类型，完成相关设置并保存

    <div style="float:left;width:100%;"><img src="http://dev.umeng.com/images/ios/create_slot.png"/></a></div><div style="clear:both"></div>

* **获取slot id** (slot id是对广告位的唯一标识，在SDK中需要使用该字段获取对应的广告信息)点击广告位列表左上方的 “获取代码” 按钮，弹出如下信息，选择相关的广告位，点击获取代码，即可在弹框右侧看到对应的slot id

    <div style="float:left;width:100%;"><img src="http://dev.umeng.com/images/ios/check_slotid.png"/></a></div><div style="clear:both"></div>

* **添加广告至广告位** 点击相关广告位右侧的 “展开广告” --> "添加广告" 完成相关设置并保存

    <div style="float:left;width:100%;"><img src="http://dev.umeng.com/images/ios/slots.png"/></a></div><div style="clear:both"></div>

  <div style="float:left;width:100%;"><img src="http://dev.umeng.com/images/ios/create_ad.png"/></div><div style="clear:both"></div>
  
### <a name="open_taobao"></a>2.1 申请淘宝开放平台账号(自定义入口样式需要申请,不集成自定义入口可以跳过)

* 首先确认联系客服将需要申请淘宝开放平台的账号加入白名单中，<a href="http://www.umeng.com/aboutus_contact">客服联系方式</a>

* **登录** http://open.taobao.com/index.htm 
  <div style="float:left;width:100%;"><img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/taobaoopen_index.jpg"/></a></div><div style="clear:both"></div>
  
* **点击加入开放平台并登录** 登录后按照引导进行开发者认证，认证完成后，您将看到如下页面，点击创建应用
  <div style="float:left;width:100%;"><img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/opentaobao_login.jpg"/></a></div><div style="clear:both"></div>  
  
* **创建应用** 填写应用名称 (应用标签必须选择无线营销)--> 申请开发测试，上传图片 --> 提交安全扫描 --> 等待对外发布
  <div style="float:left;width:100%;"><img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/taobaoopen_create_app_01.jpg"/></a></div><div style="clear:both"></div>  

  <div style="float:left;width:100%;"><img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/taobaoopen_create_app_02.jpg"/></a></div><div style="clear:both"></div>  

  <div style="float:left;width:100%;"><img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/opentaobao_create_app_03.jpg"/></a></div><div style="clear:both"></div>

* **Android应用认证** 申请完成后，按照下图，点击Android应用认证，填写758665872，保存
  <div style="float:left;width:100%;"><img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/open_taobao_android_sign.jpg"/></a></div><div style="clear:both"></div>


## 3. SDK的集成

### 3.1 导入SDK 所需 jar包
下载最新版SDK的zip包，将其中的jar包解压到本地工程`libs`子目录下。
> `Eclipse`用户右键工程根目录，选择`Properties -> Java Build Path -> Libraries`，然后点击`Add External JARs...` 选择指向jar的路径，点击`OK`，即导入成功。

> **注意** 
>
> Eclipse ADT 17 以上版本用户，请在工程目录下建一个文件夹`libs`，把jar包直接拷贝到这个文件夹下，再在Eclipse里面刷新一下工程就好了。不要通过上述步骤手动添加jar包引用。 详情请参考[Dealing with dependencies in Android projects](http://tools.android.com/recent/dealingwithdependenciesinandroidprojects).

### 3.2 添加资源文件
将SDK提供的`res`文件夹拷入工程目录下, 和工程本身`res`目录合并。 

> **提示** 
>
> SDK提供的资源文件都以`umeng_`或`munion_`开头。


### 3.3 添加渠道 (可选：UFP用户按渠道投放广告)
在`<application>`中添加

```
<meta-data android:value="xxxxxxxx" android:name="UMENG_CHANNEL"></meta-data>
```

### 3.4 添加访问权限

```
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_SETTINGS" />
```

### 3.5 添加服务
打开`AndroidManifest.xml`, 在`<application>`标签中声明SDK用到的下载服务:

> 注意：com.umeng包名可能有变，如果不能下载，请检查包名，替换成正确的包名。

```
 <!-- 下载服务 -->
 <service
     android:name="com.umeng.common.ufp.net.DownloadingService"
     android:exported="true"
     android:process=":DownloadingService" >
 </service>
```

添加应用详情页，如果不添加将使用弹窗方式打开

```
 <!-- 应用详情页 -->
  <activity
            android:name="com.umeng.newxp.view.UMDetail"
            android:configChanges="keyboard|orientation"
            android:launchMode="standard" />
```


添加推广信息打开二跳墙功能，如果不添加出现二条推广点击将提示“无法打开页面”（需要android-support-v4.jar支持）


```
        <!-- 应用墙 -->
        <activity
            android:name="com.umeng.newxp.view.handler.umwall.UMWall"
            android:configChanges="keyboard|orientation"
            android:screenOrientation="portrait"
            android:theme="@style/StyledIndicators" />
        <!-- 电商墙 -->
        <activity
            android:name="com.taobao.munion.ewall.EWallContainerActivity"
            android:configChanges="keyboard|orientation"
            android:screenOrientation="portrait"
            android:theme="@style/StyledIndicators" />
        <!-- 城市切换选择页，用于团购类页面 -->
        <activity
            android:name="com.umeng.newxp.view.UMCity"
            android:configChanges="keyboard|orientation"
            android:screenOrientation="portrait" />
```



下面是一个完整的`AndroidManifest.xml`文件的例子。

```
 <?xml version="1.0" encoding="utf-8"?>
 <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.umeng.example"
    android:versionCode="1"
    android:versionName="1.0" >
    <application
        android:debuggable="true"
        android:icon="@drawable/icon"
        android:label="@string/app_name" >                
        <meta-data
            android:name="UMENG_CHANNEL"
            android:value="Android market" />
        <activity
            android:name="com.umeng.newxp.view.UMDetail"
            android:configChanges="keyboard|orientation"
            android:launchMode="standard" />
        <!-- 应用墙 -->
        <activity
            android:name="com.umeng.newxp.view.handler.umwall.UMWall"
            android:configChanges="keyboard|orientation"
            android:screenOrientation="portrait"
            android:theme="@style/StyledIndicators" />
        <!-- 电商墙 -->
        <activity
            android:name="com.taobao.munion.ewall.EWallContainerActivity"
            android:configChanges="keyboard|orientation"
            android:screenOrientation="portrait"
            android:theme="@style/StyledIndicators" />
        <!-- 城市切换选择页，用于团购类页面 -->
        <activity
            android:name="com.umeng.newxp.view.UMCity"
            android:configChanges="keyboard|orientation"
            android:screenOrientation="portrait" />
        <!-- 声明SDK用到的下载服务 -->
        <service
            android:name="com.umeng.common.ufp.net.DownloadingService"
            android:exported="true"
            android:process=":DownloadingService" >
        </service>
    </application>
    <uses-sdk android:minSdkVersion="4" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.WRITE_SETTINGS" />
</manifest>
```

### 3.6 显示推广应用

在需要添加友盟推广的`Activity`的`onCreate()`函数中添加：

```
public class BannerExample extends Activity {
    /** Called when the activity is first created. */
    @Override
    public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.banner_activity);      
    // 找到一个添加banner 的父亲节点将 banner View附着到这个节点上
    ViewGroup parent = (ViewGroup)this.findViewById(R.id.parent);      
    /* 注意替换正确的 slot_id */
        ExchangeDataService service = new ExchangeDataService("slot_id");
        ExchangeViewManager viewMgr = new ExchangeViewManager(this, service);
        viewMgr.addView(parent, ExchangeConstants.type_standalone_handler);
    }
}
```

下面的例子是对应于上面的BannerExample Activity的布局文件(banner_activity.xml)。

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent" android:layout_height="fill_parent"
    android:background="@android:color/darker_gray"
    android:id="@+id/parent">
</RelativeLayout>
```

## 4 选择显示样式

下载面列出了友盟UFP SDK所支持的所有样式。

### 1. 横幅(banner)

```
ExchangeDataService service = new ExchangeDataService("slot_id");
ExchangeViewManager viewMgr = new ExchangeViewManager(context, service);
viewMgr.addView(parent, ExchangeConstants.type_standalone_handler);
```

|                         |                                 |
|:------------------------:|:------------------------------------:|
| <img src="http://dev.umeng.com/images/android/image001.png" width="300" height="400">   | <img src="http://dev.umeng.com/images/android/image002.png" width="300" height="400"> |
| 图6-1 banner广告 | 图6-2 广告细节  |


### 2. WAP页面

2.1本地自定义入口图片

```
Drawable drawable = context.getResources().getDrawable(R.drawable.drawable_id);
new ExchangeViewManager(context, new ExchangeDataService("slot_id"))
             .addView (ExchangeConstants.type_wap_style, imageview, drawable);
```

|                         |                                 |
|:------------------------:|:------------------------------------:|
| <img src="http://dev.umeng.com/images/android/image003.png" width="300" height="400">   | <img src="http://dev.umeng.com/images/android/image004.png" width="300" height="400"> |
| 图7-1 WAP 广告   | 图7-2 广告展示  |

2.2动态入口图片

在需要展示小把手的Activity 样式文件添加一个ImageView ，添加宽度，高度，图片等属性：
<br><span style="font-weight: bold">注意：</span>该样式不需要在ImageView中指定图片。但必须在云端配置图片，否则将不显示

```
ImageView imageview = (ImageView) findViewById(R.id.image_view_id);
new ExchangeViewManager(context, new ExchangeDataService("slot_id"))
             .addView (ExchangeConstants.type_wap_style, imageview);
```

2.3直接弹出Wap页

```
new ExchangeViewManager(context, new ExchangeDataService("slot_id")).addView(null, ExchangeConstants.type_cloud_full);
```

### 3. 自定义入口
* 请联系我们的客服，将您的淘宝账号加入到我们的白名单中<a href="http://www.umeng.com/aboutus_contact">客服</a>

* 确认使用上一步加入白名单的淘宝账号到<a href="http://open.taobao.com/index.htm">淘宝开放平台</a>申请加入，并创建应用，<a href="#open_taobao">点击查看创建流程</a> 

* 自定义入口已升级，需要类库 'android-support-v4.jar'，并且需要在Manifest文件中注册“应用墙”Activity.

* 确认注册了以下Activity,其中EwallContainerActivity 需要增加data标签,其中host="oauth.m.taobao.com" android:pathPattern="/callback*" 固定填写,android:scheme为btaobao开头，加之前注册开放平台并创建应用的appkey,如android:scheme="btaobao21736666"

```
        <!-- 应用墙 -->
        <activity
            android:name="com.umeng.newxp.view.handler.umwall.UMWall"
            android:configChanges="keyboard|orientation"
            android:screenOrientation="portrait"
            android:theme="@style/StyledIndicators" />
        <!-- 电商墙 -->
        <activity
            android:name="com.taobao.munion.ewall.EWallContainerActivity"
            android:configChanges="keyboard|orientation"
            android:screenOrientation="portrait"
            android:launchMode="singleTask"
            android:exported="true"            
            android:theme="@style/StyledIndicators">
            
            <data
                    android:host="oauth.m.taobao.com"
                    android:pathPattern="/callback*"
                    android:scheme="btaobao21736666" />
                    
        </activity>
        <!-- 城市切换选择页，用于团购类页面 -->
        <activity
            android:name="com.umeng.newxp.view.UMCity"
            android:configChanges="keyboard|orientation"
            android:screenOrientation="portrait" />
```
* 电商墙效果

|                         |                                 |
|:------------------------:|:------------------------------------:|
| <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/tbwall01.png" width="250" height="400">   | <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/tbwall02.png" width="250" height="400"> |
|  图8-1 电商墙样式   | 图8-2 电商墙搜索  |


* 应用墙效果

|                         |                                 |
|:------------------------:|:------------------------------:|
| <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/appwall01.png" width="250" height="400">   | <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/appwall02.png" width="250" height="400"> |
|  图8-1 精品推荐样式1   | 图8-2 精品推荐样式2  |

3.1动态图标

在需要展示小把手的Activity 样式文件添加一个ImageView ，添加宽度，高度，图片等属性：
<br><span style="font-weight: bold">注意：</span>该样式不需要在ImageView中指定图片。但必须在云端配置图片，否则将不显示

```
ExchangeConstants.MTOP_APPKEY = <appkey(开放平台appkey)>;
ExchangeConstants.MTOP_APP_SECRET = <appsecret(开放平台appsecret)>;
ExchangeConstants.MTOP_APP_SIGNATURE = <sign(android应用证书)>;
View imageview = findViewById(R.id.rlayout1);
new ExchangeViewManager(context, new ExchangeDataService(slot_id))
                 .addView(ExchangeConstants.type_list_curtain, imageview);
```

MTOP_APPKEY为开放平台appkey  
MTOP_APP_SECRET为开放平台appsecret  
MTOP_APP_SIGNATURE为开放平台android应用认证，默认为758665872  

appkey和appsecret在开放平台中，应用设置-->应用证书中查看，如下图

<div style="float:left;width:100%;"><img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/open_ certificate.tiff"/></a></div><div style="clear:both"></div>

1.在需要添加入口的布局文件中添加如下布局信息

```

      <!-- 入口布局 -->
    <RelativeLayout
        android:id="@+id/rlayout1"
        android:layout_width="40dp"
        android:layout_height="70dp"
        android:layout_alignParentLeft="true"
        android:layout_centerVertical="true" >

        <!-- 入口点击图标 -->
        <ImageView
            android:id="@+id/imageview"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_alignParentBottom="true"
            android:layout_alignParentLeft="true"
            android:layout_marginRight="5dp"
            android:scaleType="fitXY" />
        
        <!-- 入口文字 -->
         <TextView
            android:id="@+id/textview"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:gravity="center"
            android:layout_marginRight="5dp"
            android:textSize="12sp"
            android:textColor="#000"/>

        <!-- 新广告提示布局 -->
       <RelativeLayout
            android:id="@+id/newtip_area"
            android:layout_width="28dp"
            android:layout_height="20dp"
            android:layout_alignParentRight="true"
            android:gravity="right" >

            <ImageView
                android:id="@+id/newtip_iv"
                android:layout_width="wrap_content"
                android:layout_height="fill_parent"
                android:layout_alignParentRight="true"
                android:scaleType="fitCenter" />

            <TextView
                android:id="@+id/newtip_tv"
                android:layout_width="20dp"
                android:layout_height="20dp"
                android:layout_alignParentRight="true"
                android:gravity="center"
                android:paddingBottom="4dp"
                android:textColor="#ffffff"
                android:textSize="8sp"
                android:textStyle="bold" />
        </RelativeLayout>
        
    </RelativeLayout>
```
   
3.2静态图标

在需要展示小把手的Activity 样式文件添加一个ImageView ，添加宽度，高度，图片等属性：

```
ExchangeConstants.MTOP_APPKEY = <appkey(开放平台appkey)>;
ExchangeConstants.MTOP_APP_SECRET = <appsecret(开放平台appsecret)>;
ExchangeConstants.MTOP_APP_SIGNATURE = <sign(android应用证书)>;
ImageView imageview = (ImageView) findViewById(R.id.image_view_id);
Drawable drawable = context.getResources().getDrawable(R.drawable.drawable_id);
new ExchangeViewManager(context, new ExchangeDataService("slot_id"))
            .addView(ExchangeConstants.type_list_curtain, imageview, drawable);            
```

MTOP_APPKEY为开放平台appkey  
MTOP_APP_SECRET为开放平台appsecret  
MTOP_APP_SIGNATURE为开放平台android应用认证，默认为758665872  

appkey和appsecret在开放平台中，应用设置-->应用证书中查看，如下图

<div style="float:left;width:100%;"><img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/open_ certificate.tiff"/></a></div><div style="clear:both"></div>

### 4. 内嵌入口

集成方式：

```
ViewGroup fatherLayout = (ViewGroup) this.findViewById(R.id.ad);
ListView listView = (ListView) this.findViewById(R.id.list);
ExchangeViewManager exchangeViewManager = new ExchangeViewManager(this,new ExchangeDataService("slot_id"));
exchangeViewManager.addView(fatherLayout, listView);    
```

对应的布局文件如下：


```
<RelativeLayout
    android:id="@+id/ad"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <ListView
        android:id="@+id/list"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:background="#00000000"
        android:cacheColorHint="#00000000"
        android:divider="#dedfde"
        android:dividerHeight="1px"
        android:listSelector="#00000000" >
    </ListView>
 </RelativeLayout>    
```

添加新广告提示

1.在入口处预加载物料。
入口Activity中：

```
preloadDataService = new ExchangeDataService("40251");
preloadDataService.preloadData(getActivity(), new NTipsChangedListener() {
    @Override
    public void onChanged(int flag) {
        //do some thing
    };
}, ExchangeConstants.type_container);
```

2.集成时使用preloadDataService 作为参数

|                         |                                 |
|:------------------------:|:------------------------------------:|
| <img src="http://dev.umeng.com/images/android/image007.png" width="300" height="400">   | <img src="http://dev.umeng.com/images/android/image008.png" width="300" height="400"> |
| 图9-1 获取广告中  | 图9-2 广告展示  |

### 5. 文字链

在需要展示文字链的Activity 样式文件添加一个RelativeLayout 作为rootView 文字链长度填充rootView,高度将按照文字指定大小显示。

```
ExchangeViewManager exchangeViewManager =
new ExchangeViewManager(context, new ExchangeDataService(soltId));
exchangeViewManager.addView(rootView, ExchangeConstants.type_hypertextlink_banner);
```

设置轮播时间，在exchangeViewManager.addView 之前添加如下，时间单位毫秒

```
exchangeViewManager-.setLoopInterval(time);
```

### 6. 轮播大图

```
//自定义一个 RelativeLayout 实例
ViewGroup parent = 实例;
ExchangeDataService service = new ExchangeDataService("slot_id");
ExchangeViewManager viewMgr = new ExchangeViewManager(context, service);
viewMgr.addView(parent, ExchangeConstants.type_large_image);
```

|                         |                                 |
|:------------------------:|:------------------------------------:|
| <img src="http://dev.umeng.com/images/android/image009.png" width="250" height="400">   | <img src="http://dev.umeng.com/images/android/image010.png" width="250" height="400"> |
| 图10-1 获取广告中  | 图10-2 广告展示  |

### 7. Push弹窗
该样式内容是以Web形式，所以加载有一定延迟。后台可配置弹窗大小。

```
ExchangeDataService es = new ExchangeDataService("slot_id");
ExchangeViewManager vMgr = new ExchangeViewManager(mContext, es);
FloatDialogConfig config = new FloatDialogConfig//可选配置
    .setTimeout(3000)//设置弹窗超时不显示时间
    .setDelay(true)//设置窗口网页加载到一定进度再弹出(setDelayProgress)
    //.setListener(pushListener)//设置Push周期回调
    .setDelayProgress(30);//设置窗口延迟弹出进度
            
vMgr.setFeatureConfig(config);
vMgr.addView(null, ExchangeConstants.type_float_dialog);
```

回调设置

```
FloatDialogListener pushListener = new XpListenersCenter.FloatDialogListener() {
            /**
             * 开始加载广告
             */
            @Override
            public void onStart() {
                Log.d("TestData", "onStart");
            }

            /**
             * 获取广告准备渲染 0 failed 1 successed
             *
             * @param status
             */
            @Override
            public void onPrepared(int status) {
                Log.d("TestData", "onPrepared " + status);

            }

            /**
             * 显示窗口
             * @param isTimeout 是否超时 超时将不显示
             */
            @Override
            public void onShow(boolean isTimeout) {
                Log.d("TestData", "onShow  " + isTimeout);
            }

            /**
             * 隐藏窗口
             */
            @Override
            public void onClose() {
                Log.d("TestData", "onClose  ");
            }

            /**
             * 推广信息被点击
             */
            @Override
            public void onClick() {
                Log.d("TestData", "onClick  ");
            }

        };
```

|                         |                                 |
|:------------------------:|:------------------------------------:|
| <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/popup01.png" width="250" height="400">   | <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/popup02.png" width="250" height="400"> |
| 图11-1 60%窗口  | 图11-2 全屏  |


### 8.开屏样式

```
    /**
        * Umeng 开屏样式集成：
        * 
        * WelcomeAdsListener：无论是否展示都会调用onFinish
        *      
        *  超时或请求数据完毕
        *          |
        * onDataReviced
        *          |-没有广告-onFinish
        *       有广告   
        *          |
        *       onShow
        *          |-发生错误-onError
        *          |                  |
        *   onCountdown    onFinish
        *         |
        *      onFinish  
        *      
        *  如果在广告加载期间启动页已经关闭，将不会回调。
        *          
        */      
        new ExchangeViewManager(this).addWelcomeAds("44751", new WelcomeAdsListener() {
            @Override
            public void onShow(View view) {
                System.out.println("onShow ");
            }
            
            @Override
            public void onFinish() {
                System.out.println("onFinish ");
                WelcomeExample.this.finish();
            }
            @Override
            public void onDataReviced(Promoter data) {
                System.out.println("onDataReviced [" + (data == null ? 0 : data.size) );
            }
            
            @Override
            public void onCountdown(int count) {
                System.out.println("onCountdown " + count);
            }

            @Override
            public void onError(String msg) {
                System.out.println("onError " + msg);
            }
        } , 1000, 3000);//广告将在调用addview 1000~3000 内展示，如果该时间段内没有接收到广告数据将调用onFinish
```

|                         |                                 |
|:------------------------:|:------------------------------------:|
| <img src="http://dev.umeng.com/images/android/image100.png" width="300" height="400">   | <img src="http://dev.umeng.com/images/android/image101.png" width="300" height="400"> |
| 图10-1 启动页  | 图10-2 广告展示  |

其他设置

* ExchangeConstants.WELCOME_COUNTDOWN = true;//设置是否显示开屏样式倒计时
* 设置开屏样式动画 'umeng_xp_cm_style.xml/umeng_xp_welcome_dialog_animation'
* 设置开屏样式窗口样式'umeng_xp_cm_style.xml/umeng_xp_welcome_dialog_style'

### 9.积分墙

* 集成方式
在AndroidManifest.xml添加Activity

```
<activity
            android:name="com.umeng.newxp.view.UMCity"
            android:configChanges="keyboard|orientation"
            android:screenOrientation="portrait" />
```

代码中添加

```
ExchangeDataService mExchangeDataService = new ExchangeDataService("52465");
mExchangeDataService.setCreditUID("umeng_test");// 设置用户唯一标识（必填）
final ExchangeViewManager exchangeViewManager = new ExchangeViewManager(
        CreditWallExample.this,
        mExchangeDataService);

root.findViewById(R.id.entrance).setOnClickListener(new OnClickListener() {
    @Override
    public void onClick(View v) {
        exchangeViewManager.addView(ExchangeConstants.type_credits_wall, null);
    }
});

```

* 积分墙效果

|                         |                                 |
|:------------------------:|:------------------------------------:|
| <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/creditwall01.png" width="250" height="400">   | <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/creditwall02.png" width="250" height="400"> |
|  图8-1 积分墙样式1   | 图8-2 积分墙样式2  |

* 积分接口

```
// =================积分查询===========================

        mExchangeDataService.queryCredits(new UMCreditQueryListener() {
            @Override
            public void onComplete(int status,int credits, String data) {
                // status -1 网络异常或服务器异常 0：失败 1：成功
                // credits 当前积分 
                // data 原始json数据
                Toast.makeText(getActivity(), status+"   当前积分：" + credits, 1).show();
            }

        });

// =================积分消费===========================

        // 100 消费100积分
        mExchangeDataService.consumeCredit(100, new UMCreditListener() {
            @Override
            public void onComplete(int status, String data) {
                // status -1 网络异常或服务器异常 0：失败 1：成功
                // data 原始json数据
                if(1 == status)
                    Toast.makeText(getActivity(), "成功消费100积分", 1).show();
                else
                    Toast.makeText(getActivity(), "发生错误："+status, 1).show();
            }
        });
```

### 10.信息流

* 集成方式

开发者需要创建一个或多个广告位，每个广告位对应一个feed

步骤1：在列表页前初始化Feed信息，没有初始化完成的Feed将无法使用

```
            FeedsManager feedsManager = new FeedsManager(getActivity());
            String slot = "46660";
            feedsManager.addMaterial(slot,slot);
            slot = "46658";
            feedsManager.addMaterial(slot,slot);
            feedsManager.incubate(); //开始孵化feed广告
            feedsManager.setIncubatedListener(new IncubatedListener() {                
                @Override
                public void onComplete(int status, Feed feed, Object tag) {
                    //孵化过程回调
//                    Toast.makeText(getActivity(), "get feed "+tag, Toast.LENGTH_SHORT).show();
                }
            });
```

步骤2：在列表页获取已经初始化成功的Feed

```
List<Feed> mGlobalFeeds = new ArrayList<Feed>();
//将初始化完成的Feed装入mGlobalFeeds
feedsManager.getProducts(mGlobalFeeds);//这里的feedsManager和步骤1中使用的FeedsManager是同一个对象
```

步骤3：将获取的Feed你设定的插入方式插入列表
1.使用FeedViewFactory生成相应的View以便插入List中

```
View feedView = FeedViewFactory.getFeedView(activity, feed);
```

2.直接使用Feed中的推广数据定制UI

```
 List<Promoter> promoters = feed.getPromoters();//获取一个feed中包含的推广信息
```

Promoter 中字段含义

| 字段变量      | 含义           | 
| ------------- |:-------------:| 
|promoter|广告的ID|
|category|流量类型(0: 交换, 1:自主 ，2:付费推广)|
|content_type|应用或网址(0:应用, 1: 网址)|
|display_type|标准或图片(0:standard, 1: image，2：hyperlink text)|
|img| Banner图或头图的URL|
|image_type|插屏广告位，增加图片类型字段（img_type）, 取值为0（方图）， 1（长图）|
|landing_type|(0: popup, 1:down, 2:webview, 3:browser,4:wap_view 91:goStore) 打开方式|
|text_font|Hyperlink text 字体|
|text_size|Hyperlink text 文字大小|
|text_color|文字颜色|
|title|名称|
|provider|开发者|
|ad_words|广告语|
|description|描述|
|icon|图标URL|
|url|地址（apk下载地址或网址或app store下载地址）|
|app_version_code|包的版本号 如136|
|url_in_app|应用内地址，用于打开应用制定页面|
|size|apk 大小|
|app_package_name|APK的包名|
|app_version_name|APK的版本名|
|new_tip|是否新广告 1 表示新广告 0 非新广告(default)|


注意：如果获取的Feed已经使用过，第二次进入想要复用之前使用过的Feed,需要调用cleanReportFlag

```
//再次使用feed必须调用cleanReportFlag，否则将不发送展示报告
feed.cleanReportFlag();            
```

Demo中给出了一个信息流样式集成的使用场景，初始化过程在XpHome.java页面中进行，使用在FeedsExample.java

* 信息流效果

|                         |                                 |
|:------------------------:|:------------------------------------:|
| <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/feed01.png" width="250" height="400">   | <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/feed02.png" width="250" height="400"> |
|  图8-1 信息流样式1   | 图8-2 信息流样式2  |

* 积分接口

```
// =================积分查询===========================

        mExchangeDataService.queryCredits(new UMCreditQueryListener() {
            @Override
            public void onComplete(int status,int credits, String data) {
                // status -1 网络异常或服务器异常 0：失败 1：成功
                // credits 当前积分 
                // data 原始json数据
                Toast.makeText(getActivity(), status+"   当前积分：" + credits, 1).show();
            }

        });

// =================积分消费===========================

        // 100 消费100积分
        mExchangeDataService.consumeCredit(100, new UMCreditListener() {
            @Override
            public void onComplete(int status, String data) {
                // status -1 网络异常或服务器异常 0：失败 1：成功
                // data 原始json数据
                if(1 == status)
                    Toast.makeText(getActivity(), "成功消费100积分", 1).show();
                else
                    Toast.makeText(getActivity(), "发生错误："+status, 1).show();
            }
        });
```






## 备注

### 1. 在非中文手机上，默认不展示推广内容。
如果想在非中文手机上显示推广内容， 在请求数据之前将ExchangeConstants.ONLY_CHINESE设置为false。

```
public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.splash_activity);
        ExchangeConstants.ONLY_CHINESE=false;
        ViewGroup fatherLayout1 = (ViewGroup) this.findViewById(R.id.tab1);
        ListView listView1 = (ListView) this.findViewById(R.id.list1);
        new ExchangeViewManager().addView(this, fatherLayout1, listView1);
}       
```

### 2. 可以通过设置下面的变量改变SDK默认的界面或者行为

>* ExchangeConstants.full_screen: 显示全屏推荐时是否隐藏系统工具栏
>* ExchangeConstants.blur_switcher: 弹出窗口后是否使用阴影遮挡其他部分
>* ExchangeConstants.ONLY_CHINESE: 是否在非中文环境下展示，默认关闭
>* ExchangeConstants.banner_alpha：如果使用standAlone模式，可设置banner的透明度
>* ExchangeConstants.TIPS_DOWNLOAD：如果使用全屏样式对notification不可见，可设置该字段下载完成会有Toast提示。
>* ExchangeConstants.PRELOAD_REPEAT_COUNT：可设置加载的广告复用次数，默认值 1（如小把手进入后退出再进入会复用上一次加载的广告）
>* 如果想要修改默认的列表元素显示样式， 可以修改文件
对于嵌入式List： exchange_container_banner.xml. 
对于置顶/底下把手：exchange_normal_banner.xml

注意:不要改变这两个文件里面元素的id， 但是可以改变他们的属性， 比如，android:visible, 字体颜色，大小等。

>* 如果想在Logcat里面打印log: <br>
&nbsp;&nbsp;&nbsp;  com.umeng.common.ufp.Log.LOG = true; <br>
&nbsp;&nbsp;&nbsp;  ExchangeConstants.DEBUG_MODE=true;

### 3. 其他接口功能介绍

#### 1.Exchange.initializeListener 数据初始化回调接口

推广数据加载回调，可用来判断推广信息是否加载成功

使用方法：

```
exDataService = ...

exDataService.initializeListener = new XpListenersCenter.InitializeListener() {
            @Override
            public void onStartRequestData(int type) {
            	//开始加载推广信息，type:广告位类型
            }

            @Override
            public void onReceived(int count) {
            	//count:成功加载推广信息的数量
            }
        };
        
```


### 4. 权限说明

<table width="100%" border="0" cellspacing="0" cellpadding="0">
  <tbody><tr>
    <th scope="col" width="300">权限</th>
    <th scope="col">用途</th>

  </tr>
  <tr>
    <td>android.permission.INTERNET</td>
    <td>允许应用程序联网，以便向我们的服务器端发送数据。</td>
  </tr>
  <tr>
    <td>android.permission.ACCESS_NETWORK_STATE</td>

    <td>获取用户手机的IMEI，用来唯一的标识用户。(如果您的应用会运行在无法读取IMEI的平板上，我们会将mac地址作为用户的唯一标识，请添加权限： android.permission.ACCESS_WIFI_STATE )</td>
  </tr>
  <tr>
    <td>android.permission.READ_PHONE_STATE</td>
    <td>检测网络状态，友盟SDK 1.6版本新增权限。</td>
  </tr>

  <tr>
    <td>android.permission.WRITE_EXTERNAL_STORAGE</td>
    <td>如果您使用了友盟自动更新提醒功能，需添加这个权限，为了将更新的APK临时存在SD卡里。</td>
  </tr>
</tbody></table>


### 5. 混淆

```
-dontwarn com.umeng.**

-dontwarn android.taobao.**

-dontwarn com.taobao.**

-keep class com.umeng.** {*;}
-keep class com.taobao.** {*; }
-keep class android.taobao.** {*; }

-keep public class [your_pkg].R$*{
    *;
}
```
> 混淆过程中遇到的问题,具体请见[这里](/faq/faq_diff_android.html?expand=1).


## 技术支持
请发邮件至<support@umeng.com>，我们会尽快回复您。 
