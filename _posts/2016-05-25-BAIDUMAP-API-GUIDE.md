---
layout: post
title:  "百度地图API使用入门"
date:   2016-05-23 10:37:52
categories: 错误集锦 经验分享
tags:	百度地图 API
author: 何帆
---

* content
{:toc}


百度地图 Android SDK是一套基于Android 2.1及以上版本设备的应用程序接口。 您可以使用该套 SDK开发适用于Android系统移动设备的地图应用，通过调用地图SDK接口，您可以轻松访问百度地图服务和数据，构建功能丰富、交互性强的地图类应用程序。



> 版权声明：本文为博主原创文章，未经博主允许不得转载。  

额。还是详细记录下百度地图API的使用过程吧，网上的方法也很多，不过就算照着百度官方的指南使用API还是有蛮多问题的。为了不误导大众，此处的方法不一定有效，请大家谨慎使用。

[SDK下载地址](http://developer.baidu.com/map/index.php?title=androidsdk/sdkandev-download)

**一、申请密钥**

使用百度地图API要申请一个密钥，申请过程也很简单，百度官方就有[指南](http://lbsyun.baidu.com/index.php?title=androidsdk/guide/key)，大家照着做就可以了，注意的是下图中打码的那串就是你的密钥。等下会讲解怎么使用密钥

**二、配置环境**

[官方指南](http://lbsyun.baidu.com/index.php?title=androidsdk/guide/buildproject#Android_Studio.E5.B7.A5.E7.A8.8B.E9.85.8D.E7.BD.AE.E6.96.B9.E6.B3.95)在这，我主要讲一下AS下的，你如果照着官方指南来配置AS下的环境，发布到设备上可能会出现能运行闪退的问题，因为有些设置是设置了host GPU渲染的，版本比较低的话，会出现无法加载地图的问题，导致闪退。我用的Genymotion 模拟器就一直都会出现这个问题，而用真机测试就不会。  

**三、创建工程**

（1）在application中添加开发密钥

    <application>  
    	<!-- value所配置的密钥，name不用改，这是SDK要求的-->
    	<meta-data  
    		android:name="com.baidu.lbsapi.API_KEY"  
    		android:value="开发者 key" />  
    </application>

（2）添加所需权限

    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="com.android.launcher.permission.READ_SETTINGS" />
    <uses-permission android:name="android.permission.WAKE_LOCK"/>
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.GET_TASKS" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.WRITE_SETTINGS" />

（3）在布局xml文件中添加地图控件：

    <com.baidu.mapapi.map.MapView  
    android:id="@+id/bmapView"  
    android:layout_width="fill_parent"  
    android:layout_height="fill_parent"  
    android:clickable="true" />

（4）创建地图Activity，管理地图生命周期：

    public class MainActivity extends Activity {  
	    MapView mMapView = null;  
	    
		@Override  
	    protected void onCreate(Bundle savedInstanceState) {  
		    super.onCreate(savedInstanceState);   
		    //在使用SDK各组件之前初始化context信息，传入ApplicationContext  
		    //注意该方法要再setContentView方法之前实现  
		    SDKInitializer.initialize(getApplicationContext());  
		    setContentView(R.layout.activity_main);  
		    //获取地图控件引用  
		    mMapView = (MapView) findViewById(R.id.bmapView);  
	    } 
 
	    @Override  
	    protected void onDestroy() {  
		    super.onDestroy();  
		    //在activity执行onDestroy时执行mMapView.onDestroy()，实现地图生命周期管理  
		    mMapView.onDestroy();  
	    }  

	    @Override  
	    protected void onResume() {  
		    super.onResume();  
		    //在activity执行onResume时执行mMapView. onResume ()，实现地图生命周期管理  
		    mMapView.onResume();  
	    }  

	    @Override  
	    protected void onPause() {  
		    super.onPause();  
		    //在activity执行onPause时执行mMapView. onPause ()，实现地图生命周期管理  
		    mMapView.onPause();  
	    }  
    }

完成以上步骤后，运行程序，即可在您的应用中显示如下地图：

![](http://ww2.sinaimg.cn/mw690/005JzrjDgw1f46zkg5suej30k00zktas.jpg)

还有个**注意点**就是，百度地图API在3.0之后有了一个很大的变动，一些类比如 `BMapManage` 已经不能使用了，官方API里也移除了，而是替换成 `BaiduMap` 这个类，网上有些教程还在用老旧的方法，导致很多人找不到帮助文档。
