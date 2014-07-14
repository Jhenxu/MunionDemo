## **<center>Android 推广 SDK使用指南</center>**

### 1. 简介： 
* 服务简介:
阿里妈妈推广SDK提供了商品推广，App自主推广，推广管理，App间交叉推广等功能。
* SDK 简介：
  * 提供 _**<font color='green'>8</font>**_ 种样式:  横幅，推广墙，内嵌墙，文字链，轮播大图，插屏，开屏，信息流

===

### 2. 建立推广位，获推广位ID

### 3. SDK的集成

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
> SDK提供的资源文件都以`taobao_`开头。


### 3.3 添加渠道 (可选：用户按渠道进行推广)
在`<application>`中添加

```
<meta-data android:value="xxxxxxxx" android:name="MUNION_CHANNEL"></meta-data>
```

### 3.4 添加访问权限

```
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.WRITE_SETTINGS" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
    <uses-permission android:name="android.permission.INTERNET" />
```

### 3.5 添加服务
打开`AndroidManifest.xml`, 在`<application>`标签中声明SDK用到的下载服务:

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
       android:name="com.taobao.newxp.view.UMDetail"
       android:configChanges="keyboard|orientation"
       android:launchMode="standard"
       android:screenOrientation="portrait" />
```

## 4 选择推广样式

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



### 3. 推广墙
* 请联系我们的客服，将您的淘宝账号加入到我们的白名单中<a href="http://www.umeng.com/aboutus_contact">客服</a>

* 需要类库 'android-support-v4.jar'，并且需要在Manifest文件中注册“应用墙”Activity.


```

        <!-- 应用墙 -->
        <activity
            android:name="com.taobao.newxp.view.handler.umwall.UMWall"
            android:configChanges="keyboard|orientation"
            android:hardwareAccelerated="true"
            android:launchMode="singleTask"
            android:screenOrientation="portrait"
            android:theme="@style/DefaultStyledIndicators" />

        <!-- 电商推广墙 -->
        <activity
            android:name="com.taobao.newxp.view.handler.umwall.TaobaoWall"
            android:configChanges="keyboard|orientation"
            android:hardwareAccelerated="true"
            android:launchMode="singleTask"
            android:screenOrientation="portrait"
            android:theme="@style/TaobaoStyledIndicators" />       
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

3.1添加入口

在需要展示推广墙的Activity 样式文件添加一个ImageView ，添加宽度，高度，图片等属性：
<br><span style="font-weight: bold">注意：</span>该样式不需要在ImageView中指定图片。但必须在云端配置图片，否则将不显示

```
AlimmContext.getAliContext().init(this);//必须保证这段代码最先执行
View view = findViewById(R.id.rlayout1);
new ExchangeViewManager(context, new ExchangeDataService(slot_id))
                 .addView(ExchangeConstants.type_list_curtain, view);
```


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

### 4. 内嵌入口

集成方式：

```
AlimmContext.getAliContext().init(this);//必须保证这段代码最先执行
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

添加新推广提示

1.在入口处预加载物料。
入口Activity中：

```
AlimmContext.getAliContext().init(this);//必须保证这段代码最先执行
preloadDataService = new ExchangeDataService("slot_id");
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
| <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/container01.png" width="300" height="400">   | <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/container02.png" width="300" height="400"> |
| 图9-1 推广展示  | 图9-2 推广展示  |

### 5. 文字链

在需要展示文字链的Activity 样式文件添加一个RelativeLayout 作为rootView 文字链长度填充rootView,高度将按照文字指定大小显示。

```
AlimmContext.getAliContext().init(this);//必须保证这段代码最先执行
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
AlimmContext.getAliContext().init(this);//必须保证这段代码最先执行
//自定义一个 RelativeLayout 实例
ViewGroup parent = 实例;
ExchangeDataService service = new ExchangeDataService("slot_id");
ExchangeViewManager viewMgr = new ExchangeViewManager(context, service);
viewMgr.addView(parent, ExchangeConstants.type_large_image);
```

|                         |                                 |
|:------------------------:|:------------------------------------:|
| <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/header_image_01.png" width="250" height="400">   | <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/header_image_02.png" width="250" height="400"> |
| 图10-1 推广展示  | 图10-2 推广展示  |


### 7.开屏

```
AlimmContext.getAliContext().init(this);//必须保证这段代码最先执行
    /**
        * 开屏样式集成：
        * 
        * WelcomeAdsListener：无论是否展示都会调用onFinish
        *      
        *  超时或请求数据完毕
        *          |
        * onDataReviced
        *          |-没有推广-onFinish
        *       有推广   
        *          |
        *       onShow
        *          |-发生错误-onError
        *          |                  |
        *   onCountdown    onFinish
        *         |
        *      onFinish  
        *      
        *  如果在推广加载期间启动页已经关闭，将不会回调。
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
        } , 1000, 3000);//推广将在调用addview 1000~3000 内展示，如果该时间段内没有接收到推广数据将调用onFinish
```

|                         |                                 |
|:------------------------:|:------------------------------------:|
| <img src="http://dev.umeng.com/images/android/image100.png" width="300" height="400">   | <img src="http://dev.umeng.com/images/android/image101.png" width="300" height="400"> |
| 图10-1 启动页  | 图10-2 推广展示  |

其他设置

* ExchangeConstants.WELCOME_COUNTDOWN = true;//设置是否显示开屏样式倒计时
* 设置开屏样式动画 'taobao_xp_cm_style.xml/taobao_xp_welcome_dialog_animation'
* 设置开屏样式窗口样式'taobao_xp_cm_style.xml/taobao_xp_welcome_dialog_style'


### 8.信息流

* 集成方式

开发者需要创建一个或多个推广位，每个推广位对应一个feed

步骤1：在列表页前初始化Feed信息，没有初始化完成的Feed将无法使用

```
			AlimmContext.getAliContext().init(this);//必须保证这段代码最先执行
            FeedsManager feedsManager = new FeedsManager(getActivity());
            String slot = "46660";
            feedsManager.addMaterial(slot,slot);
            slot = "46658";
            feedsManager.addMaterial(slot,slot);
            feedsManager.incubate(); //开始孵化feed推广
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
|promoter|推广的ID|
|category|流量类型(0: 交换, 1:自主 ，2:付费推广)|
|content_type|应用或网址(0:应用, 1: 网址)|
|display_type|标准或图片(0:standard, 1: image，2：hyperlink text)|
|img| Banner图或头图的URL|
|image_type|插屏推广位，增加图片类型字段（img_type）, 取值为0（方图）， 1（长图）|
|landing_type|(0: popup, 1:down, 2:webview, 3:browser,4:wap_view 91:goStore) 打开方式|
|text_font|Hyperlink text 字体|
|text_size|Hyperlink text 文字大小|
|text_color|文字颜色|
|title|名称|
|provider|开发者|
|ad_words|推广语|
|description|描述|
|icon|图标URL|
|url|地址（apk下载地址或网址或app store下载地址）|
|app_version_code|包的版本号 如136|
|url_in_app|应用内地址，用于打开应用制定页面|
|size|apk 大小|
|app_package_name|APK的包名|
|app_version_name|APK的版本名|
|new_tip|是否新推广 1 表示新推广 0 非新推广(default)|


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



## 备注

### 1. 可以通过设置下面的变量改变SDK默认的界面或者行为

>* ExchangeConstants.full_screen: 显示全屏推荐时是否隐藏系统工具栏
>* ExchangeConstants.blur_switcher: 弹出窗口后是否使用阴影遮挡其他部分
>* ExchangeConstants.ONLY_CHINESE: 是否在非中文环境下展示，默认关闭
>* ExchangeConstants.banner_alpha：如果使用standAlone模式，可设置banner的透明度
>* ExchangeConstants.TIPS_DOWNLOAD：如果使用全屏样式对notification不可见，可设置该字段下载完成会有Toast提示。

注意:不要改变这两个文件里面元素的id， 但是可以改变他们的属性， 比如，android:visible, 字体颜色，大小等。


### 2. 权限说明

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
    <td>检测网络状态</td>
  </tr>

  <tr>
    <td>android.permission.WRITE_EXTERNAL_STORAGE</td>
    <td>将更新的APK临时存在SD卡里。</td>
  </tr>
</tbody></table>


### 3. 混淆

```
-dontwarn android.taobao.**
-dontwarn com.taobao.**

-keep class com.taobao.** {*; }
-keep class android.taobao.** {*; }

-keep public class [your_pkg].R$*{
    *;
}
```

## 技术支持
请发邮件至<mobilesupport@list.alibaba-inc.com>，我们会尽快回复您。 




