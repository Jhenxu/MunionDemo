## **<center>Android 交换网络 SDK使用指南</center>**

### 1. 简介： 
* 服务简介:
交换网络SDK提供了App自主推广，广告管理，App间交叉推广等功能。
* SDK 简介：略

===


### 2. 建立广告位，获取appkey


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


### 3.3 添加appkey,渠道
在`<application>`中添加

```
<!-- 添加渠道信息 -->
<meta-data android:value="xxxxxxxx" android:name="UMENG_CHANNEL"></meta-data>
<!-- 添加appkey -->
<meta-data android:value="xxxxxxxx" android:name="UMENG_APPKEY"></meta-data>
```

_**<font color=‘#green'>注意：</font>**_ 如果Manifest文件中已经添加了Umeng其他产品的Appkey,交换网络SDK的appkey可以在代码中添加，避免和其他产品appkey冲突

只使用一个交换网络appkey添加方式：

```
  ExchangeConstants.APPKEY = "xxxxxxxxxxxxx";
```
使用多个交换网络appkey:

```
ExchangeDataService mExService = new ExchangeDataService();
mExService.appkey = "xxxxxxxxxxxxxx";
```

### 3.4 添加访问权限

```
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
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
        <meta-data 
        	android:value="xxxxxxxx" 
        	android:name="UMENG_APPKEY"></meta-data>
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
</manifest>
```

### 3.6 显示推广应用

在需要添加友盟推广的`Activity`的`onCreate()`函数中添加：
* 自定义入口已升级，需要类库 'android-support-v4.jar'，并且需要在Manifest文件中注册“应用墙”Activity.

* 确认注册了以下Activity

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

* 电商墙效果(暂未开放)

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
View entrancheVp = (ImageView) findViewById(R.id.image_view_id);
Drawable drawable = context.getResources().getDrawable(R.drawable.drawable_id);
new ExchangeViewManager(context, new ExchangeDataService())
            .addView(ExchangeConstants.type_list_curtain, entrancheVp, drawable); 
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

### 2. 可以通过设置下面的变量改变SDK默认的界面或者行为。

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


### 3. 权限说明

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


### 4. 混淆

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
