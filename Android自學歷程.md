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
    [详细解释][2]
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
        
* #### BoardcastReceiver 广播接收器

    `你的应用可以使用它对外部事件进行过滤只对感兴趣的外部事件(如当电话呼入时，或者数据网络可用时)进行接收并做出响应。广播接收器没有用户界面。然而，它们可以启动一个activity或serice 来响应它们收到的信息，或者用NotificationManager 来通知用户。通知可以用很多种方式来吸引用户的注意力──闪动背灯、震动、播放声音等。一般来说是在状态栏上放一个持久的图标，用户可以打开它并获取消息。`
    
    * 广播类型
        * 普通广播，通过Context.sendBroadcast(Intent myIntent)发送的广播
        * 有序广播，通过Context.sendOrderedBroadcast(intent,receiverPermission)发送的，该方法第二个参数决定该广播的级别，同级别接受的先后是随机的，再到级别低的接受广播，高级别的或者同级别的先接收到的广播可以通过abortBroadcast()方法截断广播使其他的接受者无法收到该广播
        * 异步广播，通过Context.sendStickBroadcast(intent myIntent)方法发送的，还有sendStickyOrderedBroadcast(Intent,resultReceiver,scheduler,initialCode,initialData,initialExtras)方法，该方法具有有序广播的特性，也有异步广播的特性；发送异步广播要:`<uses-permission android:name="android.permission.BROADCAST_STICKY"/>`权限，接收并处理完Intent后，广播依然存在，知道你调用removeStickyBroadcast(intent)主动把它去掉
        * 注意，发送广播时的intent的参数与Context.startActivity()启动起来的intent不同，前者可以被多个订阅它的广播接收器调用，后者只能被一个(Activity或者Service)调用
        
    * 监听广播Intent步骤
    
        * 首先：写一个继承BroadCastReceiver的类,重写onReceive()方法,广播接收器仅在它执行这个方法时处于活跃状态。当onReceive()返回后，它即为失活状态,注意:为了保证用户交互过程的流畅,一些费时的操作要放到线程里,如类名SMSBroadcastReceiver
        * 然后：注册该广播接收者,注册有两种方法程序动态注册和AndroidManifest文件中进行静态注册（可理解为系统中注册）如下：
            *  静态注册,注册的广播，下面的priority表示接收广播的级别"2147483647"为最高优先级
            `<receiver android:name=".SMSBroadcastReceiver" >`
　             ` <intent-filter android:priority = "2147483647" >`
　　　              `<action android:name="android.provider.Telephony.SMS_RECEIVED" />`
　　            `</intent-filter>`
           ` </receiver >`

            * 动态注册，一般在Activity可交互时onResume()内注册BroadcastReceiver
            

        IntentFilter intentFilter=new IntentFilter("android.provider.Telephony.SMS_RECEIVED");
        registerReceiver(mBatteryInfoReceiver ,intentFilter);
        //反注册
        unregisterReceiver(receiver);
        
* 注意：
        
        1.广播生命周期只有十秒左右，如果在 onReceive() 内做超过十秒内的事情，就会报ANR(Application No Response) 程序无响应的错误信息，如果需要完成一项比较耗时的工作 , 应该通过发送 Intent 给 Service, 由Service 来完成 . 这里不能使用子线程来解决 , 因为 BroadcastReceiver 的生命周期很短 , 子线程可能还没有结束BroadcastReceiver 就先结束了 .BroadcastReceiver 一旦结束 , 此时 BroadcastReceiver 的所在进程很容易在系统需要内存时被优先杀死 , 因为它属于空进程 ( 没有任何活动组件的进程 ). 如果它的宿主进程被杀死 , 那么正在工作的子线程也会被杀死 . 所以采用子线程来解决是不可靠的
        
        2. 动态注册广播接收器还有一个特点，就是当用来注册的Activity关掉后，广播也就失效了。静态注册无需担忧广播接收器是否被关闭,只要设备是开启状态,广播接收器也是打开着的。也就是说哪怕app本身未启动,该app订阅的广播在触发时也会对它起作用
        
        系统常见广播Intent,如开机启动、电池电量变化、时间改变等广播
            
* #### Content Provider
    
### ListView的基本使用与优化

[参考博客][3]

* #### 基本使用
    * 职责
    （1）将数据填充到布局。
    （2）处理用户的选择点击等操作。
    * 创建需要：
    （1）ListView中的每一列的View。
    （2）填入View的数据或者图片等。
    （3）连接数据与ListView的适配器。
* ####　适配器
    配器是一个连接数据和AdapterView（ListView就是一个典型的AdapterView，后面还会学习其他的）的桥梁，通过它能有效地实现数据与AdapterView的分离设置，使AdapterView与数据的绑定更加简便，修改更加方便

    * 常用的适配器
    

| Adapter             | 含义                                |
| ------------------- | ----------------------------------- |
| ArrayAdapter<T>     | 用来绑定一个数组，支持泛型操作      |
| SimpleAdapter       | 用来绑定在xml中定义的控件对应的数据 |
| SimpleCursorAdapter | 用来绑定游标获取的数据              |
| BaseAdapter         | 通用的基础适配器                    |

* #### ListView使用BaseAdapter与ListView的基本优化

    当系统开始绘制ListView的时候，首先调用getCount()方法。得到它的返回值，即ListView的长度。然后系统调用getView()方法，根据这个长度逐一绘制ListView的每一行。也就是说，如果让getCount()返回1，那么只显示一行。而getItem()和getItemId()则在需要处理和取得Adapter中的数据时调用。

    一般调用方式：
    * 在布局文件中定义一个ListView：layoutlistview
    * 定义一个该ListView的item的布局文件：layoutItem
    * 定义一个ViewHolder用于存储每个Item中包含的控件
    * 创建一个类，继承BaseAdapter，并覆写以下方法
        * getCount():用于返回列表的总项数，决定了list有多少项
        * getItem(int position):用于返回列表当前项的数据，在处理响应方法时调用
        * getItemId(int position):用于返回长整型的索引，在处理响应方法时调用
        * getView(int position,View convertView,ViewGroup parent):返回当前项对应视图
    * 写getView()就可以了
    
    ==代码示例==：
    
    
            @Override
            public View getView(int position, View convertView, ViewGroup parent) {
                //用于显示的视图
                View v = null;
                //视图存储
                MHodler mHodler = null;

                //是否有缓存的视图
                if (convertView != null) {
                    v = convertView;
                    mHodler = (MHodler) v.getTag();
                } else {
                    //使用item的布局初始化视图
                    v = View.inflate(getActivity(), R.layout.layout_list_exams, null);
                    mHodler = new MHodler();
                    //初始化视图中每个控件
                    mHodler.iv = (ImageView) v.findViewById(R.id.iv_exam);
                    mHodler.tv_name = (TextView) v.findViewById(R.id.tv_exam_name);
                    mHodler.tv_time = (TextView) v.findViewById(R.id.tv_exam_time);
                    mHodler.tv_status = (TextView) v.findViewById(R.id.tv_exam_status);
                    //存储视图
                    v.setTag(mHodler);
                }
                //为当前视图中控件填充数据
                Exam exam = exams.get(position);
                mHodler.iv.setImageResource(R.mipmap.exam);
                mHodler.tv_name.setText(exam.name);
                mHodler.tv_time.setText(exam.begainTime + "开始\n" + exam.endTime + "结束");
                //mHodler.tv_status.setText(exam.status == 1 ? "未参加" : "已完成");
                if (exam.status == 1) {
                    mHodler.tv_status.setText( "未参加");
                    mHodler.tv_status.setTextColor(Color.RED);
                } else {
                    mHodler.tv_status.setText( "已完成");
                    mHodler.tv_status.setTextColor(Color.GREEN);
                }
                return v;
            }

* #### 注意：
    如果一个屏幕正好可以有size个item，ListView就一直持有size+1个item，因为屏幕最多显示size+1个item，滑动时，清空item，填充新的数据，item是从缓存中获取
* #### 常用的监听事件
项目被点击：
    
    lv_exams.setOnItemClickListener(new AdapterView.OnItemClickListener() {
          @Override
          public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
            //处理代码
            }
     });

* #### ListView实现加载时候显示动画

==TODO==

* #### ListView 实现拖拽

==TODO==

* #### ListView下拉刷新

==TODO==

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
![enter description here][4]

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
  [2]: http://www.cnblogs.com/bravestarrhu/archive/2012/05/02/2479461.html
  [3]: http://www.cnblogs.com/noTice520/archive/2011/12/05/2276379.html
  [4]: ./images/1464830541784.jpg "1464830541784.jpg"