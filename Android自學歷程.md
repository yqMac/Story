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
      
         ==context.startService(Intent myService)-->onCreate()-->onStartCommand(,,)-->service running -->context.stopService(intent)-->onDestroy()-->Service stop==
    
         注意:如果Service已经运行，则不会再执行Create，而直接执onStartCommand()，所以onStart在一个Service的生命周期中可能会执行多次。    

         注意:你永远都不可能直接调用startCommand（），都是通过Intent中介
    
         startService时需要参数为intent，所以可以使用intent进行Activity与Service之间传值
    
        bindService启动的服务：   
        
        ==context.bindService(p1,p2,p3)-->onCreate()-->onBind(Intent)-->Service running -->onUbind()-->onDestroy()-->service stop==
        
        注意:onBind只能执行一次，不可多次绑定。即整个Service的生命周期中，除了onStartCommand()可以通过多次调用startService多次调用，其余的onCreate,onBind,onUnbind,onDestroy等在一个生命周期中只能被调用一次。
    * 传值通信 
        startCommand方式
        
        如果service没有提供绑定功能，传给startService()的intent是应用组件与service之间唯一的通讯方式．然而，如果你希望service回发一个结果，那么启动这个service的客户端可以创建一个用于广播(使用getBroadcast())的PendingIntent然后放在intent中传给service，service然后就可以使用广播来回送结果
        
        bindService方式
        
        可以通过bindService(p1,p2,p3);的intent传值给service；
        或者，在Bindservice时候会有一个IBinder返回。可以利用它与Service进行通信：如下
        
            1、创建一个Service、S1
            
            2、Service内创建一个用于通信的接口、I1，要继承IBinder接口。
            
            3、Service内创建一个用于通信的类C1，类继承Binder实现接口I1
            
            4、在Service S1的onBind方法中，返回一个C1的实例用于通信
            
            5、在Activity中BindService的connection的回调中，将Ibinder参数强转为接口I1并保存，然后就可以使用该对象与Service S1通信了。
            
            6、在Activity中任意位置都可以使用5中获取到的Ibinder对象进行实时通信了。
            
        
    
    * 停止服务
    
        *  使用startService启动的服务需要使用StopService(intent)或者stopself
    
        一个＂启动的＂service必须管理其自己的生命期．这表示，系统不会停止或销毁这种service，除非内存不够用了并且service在onStartCommand()返回后会继续运行．所以，service必须调用stopSelf()停止自己或由另一个组件调用stopService()来停止它．
        
        一旦通过stopSelf()或stopService()发出了停止请求，系统就会尽可能快地销毁service．
    
        然而，如果你的service同时处理多个对onStartCommand()的请求，那么你不应在处理完一个请求之后就停止service，因为你可能已经又收到了新的启动请求(在第个完成后停止将会结束掉第二个)．要避免这个问题，你可以使用stopSelf(int)来保证你的停止请求对应于你最近的开始请求．也就是，当你调用stopSelf(int)时，你传递开始请求的ID(传递给onStartCommand()的startId)给service，如果service在你调用stopSelf(int)之前收到一了个新的开始请求，发现ID不同，于是service将不会停止．
    
        即在service执行结束后，使用stopself(startId)来结束自己当前服务id对应的服务。防止终止其余正在使用当前服务的操作。
    
        注意：你的应用在完成工作后停止它所有的service是非常重要的．这可以避免浪费系统资源和消耗电量．如果需要，其它的组件可以调用stopService()停止service．即使你为service启用了绑定，你也必须自己停止service，甚至它收到了对onStartCommand()的调用也这样．

        * 使用 BindService启动的服务，在Activity停止或者调用UnbindService的时候会自动停止
        
    * 服务的级别
    
        进程的优先级：
        * Empty process--仅用于缓存，提高打开速度，没有任何活动的应用组件（Activity和Service）的进程
         
        * Background process--拥有一个对于用户不可见的Activity的进程(onStop)   
        
        * Service process --拥有startService启动的服务的进程
        
        * Visible process --有一个不在前台但是用户可见的Activity(onPause)或者绑定了一个可见的Activity的Service的进程
        
        * Foreground process -- onResume的Activity，Service绑定正在操作的Activity，，有运行在前台的服务，有一个服正在执行生命周期，有一个正在执行onReceive的BroadcastReceiver
        
    
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