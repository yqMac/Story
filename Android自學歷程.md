---
title:Android自学历程
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---


本文是本人 ==Rookie(yqmac163@163.com)== 自学期间，根据网上资料整理的进度表，依次进行学习、丰富使用。


# Android 初级知识

### Activity的生命周期

 * 首先是经典的Activity生命周期图
 ![Activity life][1]

 * Activity其实是一个继承了ApplicationContext类的类，其中有与生命周期有关的七个方法:
  `protected void onCreate(Bundle savedInstanceState);`
  `protected void onStart();`
  `protected void onResume();`
  `protected void onPause();`
  `protected void onRestart();`
  `protected void onStop();`
  `protected void onDestroy();`

  * Start 启动时，将正常运行
      * onCreate()
      * onStart()
      * onResume()
  * BACK 运行中的程序，正常Back退出
      * onPause()
      * onStop()
      * onDestroy()
  * HOME 运行中的程序，HOME键临时切换出程序
      * onPause
      * onStop()
  * HOME后再次启动
      * onRestart()
      * onStart()
      * onResume()

  `需要注意的是，当HOME键切换出应用的时候，应该在onPause方法中将数据保存在变量中，在onRestart方法中重新加载保存的数据`

### Android 的四大組件与生命周期

* #### 四大组件是什么

    基础的，四大组件为: ==Activity , Service服务 Content Provider 内容提供者，BroadcastReceiver广播接收器== `

* #### Activity 

    * Activity 
        Activity ：在应用程序中，一个Activity就是一个单独的屏幕，他可以显示一些控件，也可以监听并处理用户的事件做出响应。
    
    * Intent 
        表示一个有效的做某事的请求
    
        Activity之间通过Intent进行通信。在Intent的描述结构中，有两个最重要的部分：==动作==和==动作对应的数据==。典型的动作类型：MAIN,VIEW,PICK,EDIT等。而动作的数据以URI的形式进行表示。
    * IntentFilter 
        描述一个Activity或者IntentReceivier能够操作哪些intent
        
        为一个Activity在AndroidManifest.xml中定义IntentFilter，能够定义该Activity能够处理哪些动作的Intent。当使用startActivity(Intent myIntent)进行Activity的跳转时，系统会在所有安装的应用程序定义的IntentFilter中查找，找到最匹配myIntent的Intent对应的Activity，新的Activity在接收到myIntent的通知后，开始运行。
        
        startActivity方法被调用时，将会触发解析myIntent的方法，这个机制提供了两个关键的好处。
        * Activities 能够重复处理不同来源的Intent消息。「如果一个Intent对应多个相同的IntentFilter，系统回弹窗让你进行选择」。
           
        * Activities 可以在任何时候被一个具有相同点的IntentFilter的新的Activity取代
        
        AndroidManifest文件中含有如下的过滤器的Activity组件为默认启动类，当程序启动时系统自动调用它。
    
    
        <intent-filter>
          <action android:name="android.intent.action.MAIN" />  
          <category android:name="android.intent.category.LAUNCHER" />  
        </intent-filter> 
    
    
**注意，在AndroidManifest中存在空的<intent-filter>[里面的东西被注释时]，app无法启动，需要将<intent-filter>一起注释掉**

* #### Service 服务

    一个Servcie就是一段长生命周期、没有用户界面、可以用来开发如监控类的程序 
    
    * 创建服务类
        
        每个服务都是一个继承了android.app.service类，并重写了其中部分方法的类。
    * 启动服务方式
        
        服务两种启动方式: `Context.startIntent(Intent myService);` 和 `Context.bindService(,,);`
        
      startService启动的服务：
    ==context.startService(Intent myService)-->onCreate()-->onStart()-->service running -->context.stopService(intent)-->onDestroy()-->Service stop==
    
    注意:在执行startService的时候，如果Service已经运行，则不会再执行Create，而直接执行Start，所以onStart在一个Service的生命周期中可能会执行多次。    

    注意:Service的onStart已经被startCommand替换掉了，周期不变
    
    
### ListView的基本使用与优化

### Acitvity的标准Intent

### Android屏幕适配基础

### Fragment完全解析

### Sqlite应用详解

# Andorid 中级知识

### Android应用程序的生命周期

### View深入了解

### Service 深入解析

### Gson

### 布局优化

### Intent传递对象的两种方法

### 异步消息处理机制完全解析

### AsyncTask 完全解析

### Android Custom Loading Deamo 做App的Loading动画

# Andorid 进阶

### Gradle

### 性能优化

### 9GAG--一个完整的开源项目

### Android 开发资源

# Andorid 设计

### Material Design

# Android 兼容库

### ActionBarSherlock

### ActionBar Compat

### NineOldAndroids

### Support V4

### NavigationDrawer 导航抽屉

### SlidingPanelLayout

### SwipeRefreshLayout

# Andorid 的一些开源库

### 镇楼图
![enter description here][2]

### Volley

### ActiveAndroid

### Retrofit

### Android-Universal-Image-Loader

### 开源项目分类汇总

# 老蒋简历中涉及的知识点

###　MVC和MVP设计模式

### 绘图和动画

### 互动效果

### 消息队列机制

### Glide 

### 推送 

### 地图


  [1]: ./images/1464799544450.jpg "1464799544450.jpg"
  [2]: ./images/1464830541784.jpg "1464830541784.jpg"