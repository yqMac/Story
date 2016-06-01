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

`基础的，四大组件为:Activity , Service服务 ，`

* ### Activity

    
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