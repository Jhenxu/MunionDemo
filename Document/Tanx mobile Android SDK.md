## **<center>Alimama Tanx移动SDK 使用指南</center>**

### 1. 简介： 
* 服务简介:
UFP提供了App自主推广，推广管理，App间交叉推广等功能。
* SDK 简介：
  * 提供 _**<font color='green'>3</font>**_ 种样式:  横幅(Banner), 插屏(Interstitial) 小把手(Handle), 

===


### 1. 建立推广位，获取Slot id
  
### <a name="open_taobao"></a>1.1 申请淘宝开放平台账号(自定义入口样式需要申请,不集成自定义入口可以跳过)

* 首先确认联系客服将需要申请淘宝开放平台的账号加入白名单中，客服联系方式:<mobilesupport@list.alibaba-inc.com>

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


## 2. SDK的集成

### 2.1 导入SDK 所需 jar包
下载最新版SDK的zip包，将其中的jar包解压到本地工程`libs`子目录下。
> `Eclipse`用户右键工程根目录，选择`Properties -> Java Build Path -> Libraries`，然后点击`Add External JARs...` 选择指向jar的路径，点击`OK`，即导入成功。



  Handler的jar目录为**alimama_sdk/alimama_handler/xxx.jar**
  
  banner和插屏的目录为**alimama_sdk/alimama_banner_Interstitial/**，其中**alimama_core/xxx.jar**为banner和handler公用，如果集成banner需要拷贝**alimama_banner/xxx.jar**，如果集成插屏在**alimama_interstitial/xxx.jar**


> **注意** 
>
> Eclipse ADT 17 以上版本用户，请在工程目录下建一个文件夹`libs`，把jar包直接拷贝到这个文件夹下，再在Eclipse里面刷新一下工程就好了。不要通过上述步骤手动添加jar包引用。 详情请参考[Dealing with dependencies in Android projects](http://tools.android.com/recent/dealingwithdependenciesinandroidprojects).

### 2.2 添加资源文件
将SDK提供的`res`文件夹拷入工程目录下, 和工程本身`res`目录合并。 

> **提示** 
>
> SDK提供的资源文件都以`umeng_`或`munion_`开头。
> Handler的资源文件在**alimama_sdk/alimama_handler/res**,
> banner和插屏目前没有样式


### 2.3 添加渠道 (可选：UFP用户按渠道投放推广)
在`<application>`中添加

```
<meta-data android:value="xxxxxxxx" android:name="UMENG_CHANNEL"></meta-data>
```

### 2.4 添加访问权限

```
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_SETTINGS" />
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
```

### 2.5 添加服务
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

```
 <!-- 下载服务 -->
 <service
     android:name="com.taobao.munion.base.download.DownloadingService"
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

### 2.6 显示推广应用

在需要添加阿里妈妈推广的`Activity`的`onCreate()`函数中添加：

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

## 3 选择显示样式

下载面列出了SDK所支持的所有样式。

### 1. 横幅(banner)

步骤1：在布局文件中添加Banner推广位

```
<com.taobao.munion.view.banner.MunionBannerView
            android:id="@+id/bannerView"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"/>
```

步骤2：在代码中设置Banner的推广位ID

```
public class BannerActivity extends Activity {
    MunionBannerView bannerView;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.banner_example);

        bannerView = (MunionBannerView) findViewById(R.id.bannerView);
        bannerView.setMunionId("58320");//设置推广位ID
    }

    @Override
    protected void onResume() {
        super.onResume();
		//重新加载推广
        if(bannerView != null){
            bannerView.load();
        }
    }

    @Override
    protected void onPause() {
        super.onPause();
        //关闭推广
        if(bannerView != null)
            bannerView.close();
    }

    @Override
    public void onBackPressed() {
        boolean interrupt = false;
        if (bannerView != null) {//通知Banner推广返回键按下，如果Banner进行了一些UI切换将返回true
                                // 否则返回false(如从 expand状态切换会normal状态将返回true)
            interrupt = bannerView.onBackPressed();
        }

        if (!interrupt)
            super.onBackPressed();
    }
}
```
|             |
|:-----------:|
| <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/banner.png" width="260" height="400">   | 
| 图6-1 banner推广 |

### 2.插屏

步骤1：在布局文件中添加插屏推广位

```
//添加到全屏的ViewGroup中
<com.taobao.munion.view.interstitial.MunionInterstitialView
        android:id="@+id/interstitialView"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:gravity="center"
        android:visibility="gone" />
```

步骤2：在代码中设置

```
public class InterstitialActivity extends Activity {
	private MunionInterstitialView interstitialView;

	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.interstitial);

		interstitialView = (MunionInterstitialView) findViewById(R.id.interstitialView);
		interstitialView.load(MainActivity.INSET_ID);//加载插屏推广

		interstitialView
				.setOnStateChangeCallBackListener(new OnStateChangeCallBackListener() {
					@Override
					public void onStateChanged(InterstitialState state) {
						switch (state) {
						case CLOSE:
							//关闭推广
							break;
						case READY:
							//仅当第一次加载完推广的时候回调
							interstitialView.show();
							break;
						default:
							break;
						}
					}
				});
	}
	
	protected void onResume(){
		super.onResume();
		if(interstitialView != null){
			interstitialView.show();
		}
	}
	
	protected void onPause(){
		super.onPause();
		if(interstitialView != null){
			interstitialView.close();
		}
	}
}

```
|             |
|:-----------:|
| <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/float.png" width="260" height="400">   | 
| 图6-1 banner推广 |

### 3. 自定义入口
* 请联系我们的客服，将您的淘宝账号加入到我们的白名单中
* ，客服邮箱: <mobilesupport@list.alibaba-inc.com>

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

        <!-- 新推广提示布局 -->
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
>* ExchangeConstants.PRELOAD_REPEAT_COUNT：可设置加载的推广复用次数，默认值 1（如小把手进入后退出再进入会复用上一次加载的推广）
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
            	//开始加载推广信息，type:推广位类型
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
    <td>检测网络状态，阿里妈妈吗SDK 1.6版本新增权限。</td>
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
请发邮件至<mobilesupport@list.alibaba-inc.com>，我们会尽快回复您。 
