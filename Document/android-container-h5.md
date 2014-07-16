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
步骤1：在布局文件中添加内嵌H5推广位

```
>//添加到内嵌H5的ViewGroup中
<com.taobao.munion.view.container.MunionContainerView
            android:id="@+id/containerView"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content" />;
```

步骤2：在代码中设置

```
MunionContainerView containerView = (MunionContainerView) findViewById(R.id.containerView);
containerView.load(ID, WIDTH, HEIGHT);// 设置推广位ID，推广位宽，推广位高
```
|             |
|:-----------:|
| <img src="https://raw.github.com/Jhenxu/MunionDemo/master/Document/images/containerH5.png" width="260" height="400">   | 
| 图6-1 banner推广 |


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




