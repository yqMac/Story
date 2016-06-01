---
title: Android从零开始
tags: Android,基础
grammar_cjkRuby: true
---

# 基础示例依次实现

### 打电话
* 功能代码`      

        String callNum = textIptlout.getEditText().getText().toString();
        Intent intent = new Intent();
        intent.setAction(Intent.ACTION_CALL);
        intent.setData(Uri.parse("tel:" + callNum));
        startActivity(intent);`
* 在AndroidManifest.xml的application节点外,添加权限
`<uses-permission android:name="android.permission.CALL_PHONE"/>`

### 发短信
* 权限 
 `     <uses-permission android:name="android.permission.SEND_SMS"/> `
* 功能代码`

         String msg_num = textlyout_msg_num.getEditText().getText().toString();  
         String msg_content = textlyout_msg_content.getEditText().getText().toString();
        //调用发送短信的api
        SmsManager sm = SmsManager.getDefault();
        //自动拆分短信
        ArrayList<String> sms = sm.divideMessage(msg_content);
        for (String s : sms) {
               //arg0 目标号码
             //短信中心号码
              //文本信息
              sm.sendTextMessage(msg_num,null,s,null,null);
        }`  

### 按钮点击事件
* public void onClick(View v ); v是触发该事件的组件

* Button对象的setOnClickListener(内部类实现接口)方式

* 布局中,设置onClick属性,在实现了Activity的类中,实现属性对应的方法

* 类实现OnClickListener接口,并实现onClick方法

### 五种布局方式

##### 线性布局

* 要记住的属性:
    * 方向:orientation="horizontal/vertical"
    *  
##### 

### LogCat

### 数据存储
* **本地内部存储**
    * 读写方式和PC端相同
    * 位置为data/data目录下的包名文件夹
    * getFileDir()自动获取包名文件夹下的files目录路径
    * getCacheDir()自动获取包名文件夹下的cache目录路径
    * 示例代码`   
     
          String username = textlyout_savedata01_user.getEditText().getText().toString();
          String password = textlyout_savedata01_password.getEditText().getText().toString();
          //把内容保存到本地
          //File f = new File("data/data/com.yqmac.it.learnandroid02/files/info.txt");
          File f = new File(getFilesDir(), "info.txt");
          FileOutputStream out = null;
          try {
               out = new FileOutputStream(f);
               out.write((username+"-"+password).getBytes());
               out.flush();
               Toast.makeText(this, "保存成功", Toast.LENGTH_SHORT).show();
           } catch (FileNotFoundException e) {
               e.printStackTrace();
           } catch (IOException e) {
                e.printStackTrace();
           } finally {
                 if (out != null) {
                        try {
                            out.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                }
            }`

* **外部存储SD卡**
    * 路径
        * 2.2之前版本,路径在/sdcard
        * 2.2-4.2版本,路径在/mnt/sdcard
        * 4.3版本,路径在/storage/sdcard0
        * 可以通过以上路径的快捷方式使用sd卡
        * Environment.getExternalStorageDirectory()返回File对象,封装了外部存储的真实路径
    * 状态
        * Environment.getExternalStorageState()返回状态,Environment.的Media开头常量
        * 常用:MEDIA_REMOVED sd卡不存在
        * 常用:MEDIA_UNMOUNTED sd卡存在但未挂载
        * 常用:MEDIA_CHECKING sd卡正在遍历
        * 常用:MEDIA_MOUNTED 可用
        * 常用:MEDIA_MOUNTED_READ_ONLY sd卡可用但只读
    * 容量
        * 用到的类Environment,StatFs
        * 注意 区分系统版本
        * 获取块大小,总块数,可用块数,来计算容量和可用量
        * 代码示例:`

              File path=Environment.getExternalStorageDirectory();
              StatFs stat = new StatFs(path.getPath());
              long blockSize= 0;
              long totalBlock= 0;
              long availableBlocks=0;
              if (android.os.Build.VERSION.SDK_INT >=    android.os.Build.VERSION_CODES.JELLY_BEAN_MR2) {
                  totalBlock = stat.getBlockCountLong();
                  availableBlocks=stat.getAvailableBlocksLong();
              }else {
                  totalBlock=stat.getBlockCount();
                  totalBlock = stat.getBlockCount();
                  availableBlocks=stat.getAvailableBlocks();
              }
              String str ="总:"+ Formatter.formatFileSize(this,totalBlock*blockSize);
              str += " 可用:" + Formatter.formatFileSize(this, availableBlocks * blockSize);
              return str;`

* **访问权限**
    * 长度由10个字符长度,[d-rwx]一共5种可能
    * 第一个字符d或者-,d表示是文件夹,-表示文件
    * rwx:读写执行
    * 后面是3组权限,每组3个字符长度
        * 第一组:文件拥有者[创建者]
        * 第二组:同组用户
        * 第三组:其他组用户

### SharedPreference
* 存储
    * 1.创建一个SharedPreference并指定文件名和权限的对象,上下文直接提供
    * 2.获取该对象的Editor
    * 3.利用该Editor对象进行put存储
    * 4.commit提交
    * 5.在data/data/包名/shared_prefs下有了文件名.xml文件
    * 代码示例:`
    
          //获取用户名密码
          String username = textlyout_savedata02_user.getEditText().getText().toString();
          String password = textlyout_savedata02_password.getEditText().getText().toString();
          //创建对象  
          SharedPreferences shp = getSharedPreferences("infos03", MODE_PRIVATE);
          //创建Editor
          SharedPreferences.Editor editor = shp.edit();
          editor.putString("username", username);
          editor.putString("password", password);
          //提交数据
          editor.commit();`
* 读取
    * 1.创建一个SharedPreference并指定文件名和权限的对象,上下文直接提供
    * 2.调用对象的getString方式直接返回数据
    * 代码示例:`
    
          //获取用户名密码
          SharedPreferences sharedPreferences = getSharedPreferences("infos03", MODE_PRIVATE);
          String username_read = sharedPreferences.getString("username", "");
          String password_read = sharedPreferences.getString("password", "");
          textlyout_savedata02_user.getEditText().setText(username_read);
          textlyout_savedata02_password.getEditText().setText(password_read);`

### XML和JSon

* **xml-pull方式存取**
    * 保存:序列化方式:XmlSerializer
        * 创建文件输出流FileOutputStream
        * 初始化序列器-生成的Stream和编码
            * XmlSerializer xs=Xml.newSerializer();
            * xm.setOutput(fileOutputStream,"utf-8");
        * 生成xml
            * startDocument-编码
            * startTag -根节点
            * 循环开始-------
                * startTag:子节点名称
                * text子节点文本
                * endTag:子节点结束
            * 循环结束
            * endTag:根节点结束
            *endDocumemnt文档结束
    * 读取:pullParser :XmlPullParser
        * 创建文件输入流FileInputStream
        * 获取xmlpull解析器,设定解析编码
            * XmlPullParser xp =Xml.newPullParser();
            * xp.setInput(fileInputStream,"utf-8");
        * 获取当前节点的事件类型getEventType()
            * End_Document
            * START_TAG
                * 文本节点可以获取节点名称和下节点文本
                * xp.getName()
                * xp.nextText()
            * END_TAG
        * 跳下一节点xp.next()


### 数据库 Sqlite

    sqlite中数据类型都是以varchar存储的,但是程序猿在使用的时候一般都会以正规的类型来设计,方便查看

* **创建基础操作类**
    * *Android上操作Sqlite需要自己创建类继承SQLiteOpenHelper来操作数据库的创建和升级*
    * 创建类并继承SQLiteHelper
    * 重写 构造,onCreatte,onUpgrade方法
        * 构造
        
              /**
              arg0: context 上下文
              arg1: 数据库名称,db结尾是好习惯
              arg2: 游标工厂CursorFactory,返回结果集,null则使用默认
              arg3: 版本,>0,用于升级
              /
              super(context, name, factory, version);
        * onCreate方法中创建表
        
* **创建数据库**
    * 创建你的基础操作类对象db
    * db.getWritableDatabase();
    `如果数据库不存在,会再创建再打开,数据库存在就会自动打开`

* **创建数据表**
    * 在你的工具类中的onCreate()方法中创建数据库表,此方法只在数据库首次创建的时候执行
    * 注意在sqlite中主键一般使用 _id 表示
    * 代码示例:
        
            db.execSQL("create table t_persons (_id integer primary key autoincrement , username varchar(20) ,password varchar(20) )");
* **数据库基本CRUD操作**
    
    * Insert
    
          db.execSQL("insert into t_person (username,password) values(?,?)" ,new Object[]{"admin","123456"});

          //封装后:
          //要插入的数据封装
          ContentValues values =new ContentValues();
          values.put("name1","value1");
          //执行插入,返回影响的条数-1表示失败,arg1为要置空的字段名
          long l = db.insert("tableName",null,values);
    * delete
    
          //封装后
          long l = db.delete("tableName","_id = ? and name = ? ",new String[]{"123","wangmoumou"});
          
    * update 类上
           
           //封装后
           //要修改的字段进行封装
           ContentValues values=new ContentValues();
           values.put("name","newName");
           //执行更新
           long l = db.update("tablename",values,"_id = ? " ,new String[] {"5"});
    * select
             
          //
          //查询获取游标,
          //arg0: 标明
          //arg1: 要查询的字段
          //select 语句
          //select 参数
          //
           /**
           * 查询获取游标,
           * arg0: String 表名
           * arg1: String[] 要查询的字段
           * arg2: String select 语句
           * arg3: String[] select 参数
           * arg4: String group by 参数
           * arg5: String having 参数
           * arg6: String order by 参数
           * arg7: String limit 参数
           */
           Cursor cursor = db.query("person",null,null,null,null,null,null,null);
           while (cursor.moveToNext()) {
              String username = cursor.getString(cursor.getColumnIndex("username"));
          }

* **SQLite的事务**

    * 开启事务
        `db.beginTransaction();`
        
    * 操作
    
    * 如果无异常 
        `db.setTransactionSuccessful();`
        
    * 出现异常 ,自动回滚
        `db.endTransactionSuccessful();`
    
### ListView

* ListView 仍然是 Item 的集合, 使用ListView显示数据,首先要设计每个View[Item]的显示样式
* 代码编写
    * 使用ListView需要使用Adapter填充,一般有两种方式
    * BaseAdapter,封装度最低,最自由
        * 写类MyAdapter继承 BaseAdapter类,并重写部分方法 
        
              //系统调用,用来获取模型层有多少总数据,重写要返回你的数据的size
              int getCount();
              //系统调用,返回的View对象作为ListView的一个条目显示在屏幕上
              getView(int position,View convertview,viewGroup parent);
              //可以不更改
              int getItemId(int position);
              //可以不更改
              getItem(int position);
        * 缓存,为了节省资源,在getView方法中提供了缓存的方式显示View
        
              //1.定义Bean方便Item中的数据的存储和显示
              /**
              * ListView的Item中组件
              */
              class MHolder {
                  public SmartImageView siv;
                  public TextView tv_title;
                  public TextView tv_detail;
                  public TextView tv_comments;
              }
              //2.判断缓存,然后更新Item中的内容
               public View getView(int position, View convertView, ViewGroup parent) {
                   View v = null;
                   MHolder mHolder = null;
                   if (convertView != null) {
                       v = convertView;
                       mHolder = (MHolder) v.getTag();
                   } else {
                       v = View.inflate(MyActivity.this, R.layout.layout_listnews, null);
                       mHolder = new MHolder();
                       mHolder.siv = (SmartImageView) v.findViewById(R.id.siv);
                       mHolder.tv_title = (TextView) v.findViewById(R.id.news_title);
                       mHolder.tv_detail = (TextView) v.findViewById(R.id.news_detail);
                       mHolder.tv_comments = (TextView) v.findViewById(R.id.news_comments);
                       v.setTag(mHolder);
                   }
                   MyNews m = list_myNews.get(position);
                   mHolder.siv.setImageUrl(m.getImage());
                   mHolder.tv_title.setText(m.getTitle());
                   mHolder.tv_detail.setText(m.getDetail());
                   mHolder.tv_comments.setText(m.getComment() + "条评论");
                   return v;
               }

    * ArrayAdapter
        * 每个Item只能处理一个文本以及其余的固定的东西
        * 代码:
            
              //获取ListView控件对象
              ListView lv = (ListView) findViewById(R.id.lv);
              //要处理的数据
              String[] strs = new String[]{"bt1", "bt2", "bt3"};
              /**
               *设置ListView的Adapter
               * ArrayAdapter的参数
               * arg0: 上下文对象
               * arg1: Item的布局文件
               * arg2: 要用来处理文本的TextView控件的 id
               * arg3: 要显示的全部文本
               */
               lv.setAdapter(new ArrayAdapter<String>(this, R.layout.layout_listnews,R.id.news_title, strs));
    
    * SimpleAdapter
        * 将所有数据封装成一个List<Map<String,Object>>对象,其中,每个Map中的全部数据就是每个Item中的全部信息
        * 代码示例:
            
              //示例的Item两部分,一个Imageview,一个TextView
              //用于存储总的数据的List
              List<Map<String, Object>> data = new ArrayList<Map<String, Object>>();
              //每个Item的数据
              Map<String, Object> map1 = new HashMap<>();
              map1.put("name", "刘德华");
              map1.put("photo", R.drawable.ic_launcher);
              //填充data
              data.add(map1);
              /**
               * 填充listView
               * SimpleAdapter的参数
               * arg0: Content 上下文
               * arg1: list<map> 要显示的全部数据
               * arg2: id  item的布局文件
               * arg3: string[] key列表
               * arg4: int[] key列表中数据对应显示在布局中的id
               */
              lv.setAdapter(new SimpleAdapter(this, data, R.layout.layout_listnews, new String[]{"name", "photo"}, new int[]{R.id.news_title, R.id.lv}));

    * ListView的显示
        `ListView lv = (ListView)findViewById(R.id.xx);`
        `lv.setAdapter(new MyAdapter());`
        
### UI刷新


* **Handler**
    * 所属包为Android.so包,不要导错
     
          //重写handleMessage方法
          private static Handler handler = new Handler(){
              public void handleMessage(Message msg){}
          }
          //调用方可以发送仅有what的空信息,或者将数据封装进msg的obj属性中

* **AsyncTask**


### 网络编程

* **总述**
    * Android系统中主要提供了两种方式来进行HTTP通信，HttpURLConnection和HttpClient  [ HttpClient已经被Google放弃啦 ] 
    * 于是乎，一些Android网络通信框架也就应运而生，比如说 AsyncHttpClient 异步Http通信
    * 再比如 Universal-Image-Loader ，它使得在界面上显示网络图片的操作变得极度简单
    * 在2013年Google I/O大会上推出了一个新的网络通信框架——Volley。Volley可是说是把AsyncHttpClient和Universal-Image-Loader的优点集于了一身，既可以像AsyncHttpClient一样非常简单地进行HTTP通信，也可以像Universal-Image-Loader一样轻松加载网络上的图片,**特别适合数据量小，通信频繁的网络**
    * okhttp
    * Retrofit + okhttp + json

* 从网络读取字节数据的方式0:
           
      //获取网络输入流中的制定编码文本
      public static String getStringFromStream(InputStream is,String charset) {
        try {
            byte[] buf = new byte[1024];
            int length = -1;
            ByteArrayOutputStream bArrayout = new ByteArrayOutputStream();
            while ((length = is.read(buf)) > 0) {
                bArrayout.write(buf, 0, length);
            }
            return new String(bArrayout.toByteArray(), Charset.forName(charset));
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
      }


* **HttpURLConnection**
    * new ->  URL openConnection() ->强转为 HttpURLConnection [设置各种参数]-> getResponseCode() ->getInputStream() -> InputStream

    * 设置参数
        * 请求方式 `setRequestMethod("GET");`//此处的参数必须是大写,must be one of [OPTIONS, GET, HEAD, POST, PUT, DELETE, TRACE]
        * 连接超时 `setConnectTimeout(8000);`
        * 读取超时 `setReadTimeout(8000);`
    
    * 请求需要在线程中进行,主线程默认不允许网络访问操作,防止卡UI
    
    * Handler 处理刷新UI的操作
    
    * GET方式提交数据,如上
    
    * Post 提交
        * HttpURLConnection 在设置完毕以上参数后添加Method为post, 添加post请求头自动添加的属性
        * 设置属性参数,例如文件类型`setRequestProperty("Content-Type","application/x-www-form-urlencoded");`
        * 设置打开输出 `HttpURLConnection setDoOutput(true);`
        * 获取输出流 `getOutputStream(); `
        * 把数据写入输出流 `out.write(str.getBytes()); `
        * 获取响应信息 ` getResponseCode()`
        * 读取返回信息,同GET
* **HttpClient 框架** `google6.0已经移除,google不推荐使用,推荐第一种基础方式,Apache提供的网络框架`
    * **GET方式获取状态和数据**
    * 创建HttpClient对象 `new DefaultHttpClient();`
    * 创建get请求对象 `HttpGet get = new HttpGet(path);` 
    * 用客户端发送GET请求 `HttpResponse response = clent.execute(get);`
    * 获取状态行 `StatusLine line = response.getStatusLine(); `
    * 获取状态码 `int code = line.getStatusCode(); `
    * 获取返回实体,流包含在实体中 `HttpEntity entity=respone.getEntity(); `
    * 根据实体获取数据
        * getContent(); [InputStream]
        * getContentEncoding();[Header]
        * getContentLength(); [long]
        * getContentType(); [Header]
    * **Post请求方式**
        * 创建HttpClient
        * 创建HttpPost
        * 创建HttpEntity 
            * 创建List<NameValuePair> data 封装要传递的数据
            * `BasicNameValuePair bnvp = new BasicNameValuePair("name",value);`
            * data.add(bnvp);
            * `new UrlEncodedFormEntity(data,"utf-8")用于提交表单,自动编码`
        * 把实体封装进post `post.setEntity(entity);`
        * 获取response `HttpResponse response = client.execute(post); `
        * 响应状态码 `response.getStatusLine().getStatuCode()==200`
        * 获取返回数据,同GET方式
* **AsyncHttpClient** `是对HttpClient 的封装的异步框架`
    * 注意,此框架不可在线程工作,因为是异步,所以需要在主线程
        * 1不使用异步，使用同步的，即SyncHttpClient。
        * 2将new JsonHttpResponseHandler(){....}这个handler的定义在你新起的线程外。
        
    * GET方式
        * 创建对象`AsyncHttpclient client = new AsyncHttpClient();`
        * 创建键值对[完全可以封装在url中,get方式]`RequestParams rp = new RequestParams(); rp.put("","") 在提交时写在构造中即可;` 
        * 发送Get 并定义内部类处理返回数据 
                        
              client.get(urlstr,rp,new AysncHttpResponseHandler(){
                    //如果参数封装在url中,可以省掉rp参数
                    //需要重写onSuccess 和 onFailure 方法来处理
              });
        * AysncHttpResponseHandler中函数参数默认byte[]
        * TextAysncHttpResponseHandler中函数参数默认String
        * JsonAysncHttpResponseHandler中函数参数默认JSonObject或者JSonArray
    * post方式
        * 同样使用RequestParams对提交数据进行封装
        * 调用client.post提交数据
 * **XUtil多线程下载**
     *![enter description here][1]

* **Volley**
    * jar包的获取
        * 源码地址:`git clone https://android.googlesource.com/platform/frameworks/volley`
        * 访问google的host:`https://github.com/racaljk/hosts`
        * 博客文章:`http://blog.csdn.net/guolin_blog/article/details/17482095`
    * 总述:
        * 首先是需要创建队列RequestQueue,处理Http的请求;RequestQueue内部的设计就是非常合适高并发的,每个Activity中创建一个队列即可
        * 然后创建Request对象,封装url,以及回调方法
        * 将request添加到队列
    * 详细介绍:
        * http://blog.csdn.net/a1042185842b/article/details/51158
        * StringRequest
        * JsonObjectRequest
        * ImageRequest
        * ImageLoader
        * NetworkImageView
        * 
    * StringRequest 的请求方式
        * GET
        
              StringRequest request = new StringRequest(Request.Method.GET, "http://www.1356789.com", new Response.Listener<String>() {
                  @Override
                public void onResponse(String response) {
                    textlyout_volley_user.getEditText().setText("999999999999" + response);
                }}, new Response.ErrorListener() {
               @Override
              public void onErrorResponse(VolleyError error) {
                   textlyout_volley_password.getEditText().setText("出错了" + error == null ? "error null" : error.getMessage());
              }
              });
        * Post

              StringRequest request = new StringRequest(Request.Method.GET, "http://www.1356789.com", new Response.Listener<String>() {
              @Override
              public void onResponse(String response) {
                textlyout_volley_user.getEditText().setText("999999999999" + response);
              }
              }, new Response.ErrorListener() {
              @Override
              public void onErrorResponse(VolleyError error) {
                textlyout_volley_password.getEditText().setText("出错了" + error == null ? "error null" : error.getMessage());
              }
               }){
              @Override
              protected Map<String, String> getParams() throws AuthFailureError {
                Map<String, String> map = new HashMap<>();
                map.put("username", "name");
                map.put("password", "pass");
                return map;
              }
              };
              queue.add(request);
        * 编码和缓存的处理  
        
              private void loadGetStr(String url) {    
                StringRequest srReq = new StringRequest(Request.Method.GET, url,new StrListener(), new StrErrListener()) {
                protected final String TYPE_UTF8_CHARSET = "charset=UTF-8";    
              // 重写parseNetworkResponse方法改变返回头参数解决乱码问题  
              // 主要是看服务器编码，如果服务器编码不是UTF-8的话那么就需要自己转换  反之则不需要    
              @Override    
              protected Response<String> parseNetworkResponse(NetworkResponse response) {    
                    try {    
                        String type = response.headers.get(HTTP.CONTENT_TYPE);    
                        if (type == null) {    
                            type = TYPE_UTF8_CHARSET;    
                            response.headers.put(HTTP.CONTENT_TYPE, type);    
                        } else if (!type.contains("UTF-8")) {    
                            type += ";" + TYPE_UTF8_CHARSET;    
                            response.headers.put(HTTP.CONTENT_TYPE, type);    
                        }    
                    } catch (Exception e) {    
              }    
              return super.parseNetworkResponse(response);    
              }};    
              
              srReq.setShouldCache(true); // 控制是否缓存    
              startVolley(srReq);    
              }  

    * JSonRequest 的请求方式

* **OkHttp**
* **Retrofit**

### 四大组件

### Activity

### 广播

### 服务

### 内容提供者

### Intent意图

### 多媒体

### 需要注意的UI控件

* EditText
    * 外部包裹TextInputEdit
* Button
    * 在drawable中定义样式,背景色,圆角,焦点颜色等
    * 在button中设置background选择样式
    * marginTop需要在button属性设置`

          <?xml version="1.0" encoding="utf-8"?>
          <selector xmlns:android="http://schemas.android.com/apk/res/android">
              <item android:state_pressed="false">
                  <shape android:shape="rectangle">
                      <corners android:radius="100dp"/>
                      <solid android:color="#aa081284"/>
                  </shape>
              </item>
              <item android:state_pressed="true">
                  <shape android:shape="rectangle">
                      <corners android:radius="100dp"/>
                      <solid android:color="#aa2733be"/>
                  </shape>
              </item>
          </selector>`  

### 第三方的控件

* SmartImageView 
    * setImageUrl(String url);即可显示网页图片
    * 默认会有本地缓存
* 


  [1]: ./images/QQ%E6%88%AA%E5%9B%BE20160414162707.jpg "QQ截图20160414162707.jpg"