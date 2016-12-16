## **<center>Tanx移动Android SDK集成指南</center>**

### 1. 简介： 
* 服务简介:
Tanx移动SDK提供了商品推广，App自主推广，推广管理，App间交叉推广等功能。
* SDK 简介：
  * 提供 _**<font color='green'>3</font>**_ 种样式：横幅、推广墙、插屏

---

### 2. 建立推广位，获取推广位ID
#### 2.1 添加应用

 * 点击“推广管理-应用管理”页面的“新建应用”按钮，按照要求填写应用的名称、平台、下载地址、类别等信息。

 * <div style="float:left;width:100%;"><img style='border:1px solid #cacaca' src="http://gtms03.alicdn.com/tps/i3/TB1DsjyFVXXXXbgXFXXC3Wn2pXX-1076-352.jpg"/></a></div><div style="clear:both"></div>
 

 * 您的应用一般会在1-2个工作日内提交成功，应用提交成功后则可基于此应用添加推广位。

 * 您可以在应用管理页面的提交状态位置查看提交状态。如果应用提交被拒绝，请根据提示的拒绝原因整改之后重新提交。

 * <div style="float:left;width:100%;"><img style='border:1px solid #cacaca' src="http://gtms04.alicdn.com/tps/i4/TB1ONtUFVXXXXcuaXXXZcECTFXX-1406-330.jpg"/></a></div><div style="clear:both"></div>



#### 2.2 建立推广位，获取推广位ID

 * 如果应用提交成功，则可以立即开始创建您的推广位！ 

 * <div style="float:left;width:100%;"><img style='border:1px solid #cacaca' src="http://gtms03.alicdn.com/tps/i3/TB10ZHyFVXXXXXWXVXXuOaOFpXX-984-260.jpg"/></a></div><div style="clear:both"></div>

 
 * 点击“推广管理-推广位管理-SDK集成推广”页面的“新建推广位”按钮选择推广位所属应用、推广位入口样式（即样式），按照要求填写完整推广位信息。

 * <div style="float:left;width:100%;"><img style='border:1px solid #cacaca' src="http://gtms02.alicdn.com/tps/i2/TB1v1HJFVXXXXXHXXXX3r5hKFXX-710-381.png"/></a></div><div style="clear:both"></div>


 * 推广位创建完毕后，即可获取推广位ID，用于SDK集成。

 * 点击“推广管理-推广位管理-SDK集成推广”页面的“获取推广位ID”按钮，在推广位ID页面复制取用即可。

 * <div style="float:left;width:100%;"><img style='border:1px solid #cacaca' src="http://gtms01.alicdn.com/tps/i1/TB1HtHyFVXXXXcKXpXXobkmGVXX-671-311.png"/></a></div><div style="clear:both"></div>


 * 新建推广位只能基于已经显示提交成功的应用。在Tanx移动平台上新建推广位所选样式、所获取的推广位ID，需要在SDK集成过程中对应好， 否则推广可能无法生效。

### 3. SDK的集成

#### 3.3 添加渠道 (可选：用户按渠道进行推广)
在`<application>`中添加

```
<meta-data android:value="xxxxxxxx" android:name="MUNION_CHANNEL"></meta-data>
```

#### 3.1 导入SDK 所需的文件

1. 将SDK文件夹下的jar文件本地工程`libs`子目录下。
2. 将 'assets/mu/'文件夹中的apk文件添加到本地工程的 'assets/mu/'下。

#### 3.4 添加访问权限
<center>alimama_demo\AndroidManifest.xml</center>
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

#### 3.5 添加服务
打开`AndroidManifest.xml`, 在`<application>`标签中声明SDK用到的下载服务:
<center>alimama_demo\AndroidManifest.xml</center>
```
 <!-- 下载服务 -->
 <service android:name="com.alimama.mobile.sdk.shell.DownloadingService">
            <intent-filter>
                <action android:name="com.alimama.mobile.sdk.download.action" />
            </intent-filter>
 </service>
```

添加应用详情页，如果不添加将使用弹窗方式打开
<center>alimama_demo\AndroidManifest.xml</center>
```
 <!-- 应用详情页 -->
 <activity
       android:name="com.alimama.mobile.sdk.shell.AlimamaDetail"
       android:configChanges="keyboard|orientation"
       android:launchMode="standard"
       android:screenOrientation="portrait" />
```

#### 3.6 初始化SDK

注意：初始化SDK必须在集成样式的页面之前，如果在首页集成样式，请重写Application，并在Application中进行初始化。
```
 MmuSDKFactory.getMmuSDK().init(getApplication());
 MmuSDKFactory.registerAcvitity(ContainerActivity.class);//注册集成样式页的Activity
```

### 4. 选择推广样式

下面列出了SDK所支持的所有样式。

#### 4.1 横幅(banner)

步骤1：添加插件apk（如：Banner_plugin-1.0.apk）到项目工程的 'assets/mu/'目录下

步骤2：在需要添加样式的布局文件中加入一个ViewGroup来给样式定位

例如：

```
<RelativeLayout
    android:layout_alignParentBottom="true"
    android:id="@+id/bannerParent"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:gravity="center_horizontal">
</RelativeLayout>
```

步骤3：在集成页的代码中添加

例如：
<center>alimama_demo\src\com\alimama\mobile\demo\BannerActivity.java</center>
```
public class BannerActivity extends Activity {
    private BannerProperties properties;
    private BannerController mController;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.banner);
        ViewGroup nat = (ViewGroup) findViewById(R.id.bannerParent);
        String slotId = "59335";//注意：该广告位只做测试使用，请勿集成到发布版app中
        setupAlimama(nat, slotId);
    }

    @Override
    public void onBackPressed() {
        boolean interrupt = false;
        if (mController != null) {// 通知Banner推广返回键按下，如果Banner进行了一些UI切换将返回true
            // 否则返回false(如从 expand状态切换会normal状态将返回true)
            interrupt = mController.onBackPressed();
        }

        if (!interrupt)
            super.onBackPressed();
    }

    private void setupAlimama(ViewGroup nat, String slotId) {
        MmuSDK mmuSDK = MmuSDKFactory.getMmuSDK();
        mmuSDK.init(getApplication());//初始化SDK,该方法必须保证在集成代码前调用，可移到程序入口处调用
        properties = new BannerProperties(slotId, nat);
        mController = (BannerController) properties.getMmuController();
        mmuSDK.attach(properties); 
    }
}
```

PS：更多设置请参考Demo中集成代码。

|             |
|:-----------:|
| <img src="http://gtms01.alicdn.com/tps/i1/TB16TjiFVXXXXavXpXXlbwZHpXX-246-399.png" width="260" height="400">   | 
| 图6-1 banner推广 |

#### 4.2 插屏


步骤1：添加插件apk（如：InsertPlugin-1.0.apk）到项目工程的 'assets/mu/'目录下

步骤2：在需要添加样式的布局文件中加入一个ViewGroup来给样式定位

例如：

--请将该布局添加到原来布局最上层，并且长宽全部填充满--

```
<RelativeLayout
        android:id="@+id/nat"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:visibility="gone"
        />        
```

步骤3：在集成页的代码中添加
<center>alimama_demo\src\com\alimama\mobile\demo\InsertActivity.java</center>
```
public class InsertActivity extends Activity{
    private InsertProperties properties;
    private InsertController mController; 
    
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);

        setContentView(R.layout.interstitial);

        ViewGroup parent = (ViewGroup) findViewById(R.id.nat);
        final String slotId = "59338";//注意：该广告位只做测试使用，请勿集成到发布版app中
        setupAlimama(parent, slotId);

        Button btLoad = (Button) findViewById(R.id.interstitialLoad);
        btLoad.setOnClickListener(new OnClickListener() {
            
            @Override
            public void onClick(View v) {
                if(mController != null){
                    mController.load(slotId);//显示插屏
                }
            }
        });
        
        Button btClose = (Button) findViewById(R.id.interstitialClose);
        btClose.setOnClickListener(new OnClickListener() {
            
            @Override
            public void onClick(View v) {
                if(mController != null){
                    mController.close();//隐藏插屏
                }
            }
        });
    }

	private void setupAlimama(ViewGroup parent, String slotId) {
        MmuSDK mmuSDK = MmuSDKFactory.getMmuSDK();
        mmuSDK.init(getApplication());//初始化SDK,该方法必须保证在集成代码前调用，可移到程序入口处调用
        properties = new InsertProperties(slotId, parent);
		mController = (InsertController) properties.getMmuController();
        mmuSDK.attach(properties); 
    }
}
```

PS：更多设置请参考Demo中集成代码。

|             |
|:-----------:|
| <img src="http://gtms01.alicdn.com/tps/i1/TB1vVvjFVXXXXaVXpXX1NcSHpXX-246-401.png" width="260" height="400">   | 
| 图6-1 banner推广 |



#### 4.3 推广墙
* 需要类库 'android-support-v4.jar'，并且需要在Manifest文件中注册“推广墙”Activity.

步骤1： 添加插件apk（如：HandlePlugin-1.0.apk）到项目工程的 'assets/mu/'目录下

步骤2：在Manifest中注册墙使用的Activity
<center>alimama_demo\AndroidManifest.xml</center>
-- 推广墙 --

```
<activity
    android:name="com.alimama.mobile.sdk.shell.AlimamaWall"
    android:configChanges="keyboard|orientation"
    android:hardwareAccelerated="true"
    android:launchMode="singleTask"
    android:screenOrientation="portrait" />
     
```

步骤3：在需要添加样式的布局文件中加入一个ViewGroup来给样式定位

例如：
```
   <RelativeLayout
        android:id="@+id/nat"
        android:layout_width="55dp"
        android:layout_height="50dp">
    </RelativeLayout>
```

步骤4：在集成页的代码中添加
<center>alimama_demo\src\com\alimama\mobile\demo\HandleActivity.java</center>
```
public class HandleActivity extends Activity{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.inset);
        ViewGroup nat = (ViewGroup) findViewById(R.id.nat);
        String slotId = "60338";
        setupAlimama(nat, slotId);
    }

    private void setupAlimama(ViewGroup nat, String slotId) {
        MmuSDK mmuSDK = MmuSDKFactory.getMmuSDK();
        mmuSDK.init(getApplication());
        HandleProperties properties = new HandleProperties(slotId,nat);
        mmuSDK.attach(properties);
    }
}
```
PS：更多设置请参考Demo中集成代码。

* 电商墙效果

|                         |                                 |
|:------------------------:|:------------------------------------:|
| <img src="http://gtms02.alicdn.com/tps/i2/TB1RtTjFVXXXXXkXpXXHBNITXXX-200-356.png" width="250" height="400">   | <img src="http://gtms04.alicdn.com/tps/i4/TB1cq6nFVXXXXacXXXXHBNITXXX-200-356.png" width="250" height="400"> |
|  图8-1 电商墙样式   | 图8-2 电商墙搜索  |


* 应用墙效果

|                         |                                 |
|:------------------------:|:------------------------------:|
| <img src="http://gtms03.alicdn.com/tps/i3/TB1m56dFVXXXXbAXVXXA0gJJXXX-720-1280.png" width="250" height="400">   | <img src="http://gtms04.alicdn.com/tps/i4/TB1uOR3FVXXXXbfapXXcJdn0pXX-576-1024.png" width="250" height="400"> |
|  图8-1 精品推荐样式1   | 图8-2 精品推荐样式2  |


### 5. 备注

#### 5.1 可以通过设置下面的变量改变SDK默认的界面或者行为

>* ExchangeConstants.full_screen: 显示全屏推荐时是否隐藏系统工具栏
>* ExchangeConstants.ONLY_CHINESE: 是否在非中文环境下展示，默认关闭
>* ExchangeConstants.TIPS_DOWNLOAD：如果使用全屏样式对notification不可见，可设置该字段下载完成会有Toast提示。

注意:不要改变这两个文件里面元素的id， 但是可以改变他们的属性， 比如，android:visible, 字体颜色，大小等。


#### 5.2 权限说明

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


#### 5.3 混淆

```
-dontwarn android.taobao.**
-dontwarn com.taobao.**
-dontwarn com.alimama.mobile.**
-dontwarn  android.app.**
-dontwarn android.support.v4.**

-keepattributes Signature
-keepattributes *Annotation*

-keep class com.taobao.** {*; }
-keep class com.alimama.mobile.**{*; }
-keep class android.taobao.** {*; }
-keep class android.support.v4.** { *; }
-keep class android.app.**{*;}
```

### 6. 技术支持
请发邮件至<mobilesupport@list.alibaba-inc.com>，我们会尽快回复您。