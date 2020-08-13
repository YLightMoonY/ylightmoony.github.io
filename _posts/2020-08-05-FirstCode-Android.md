---
title: 第一行代码-Android 笔记
layout: post
---

# 第一行代码-Android

*仅用于记录需要记忆的知识,一些很简单的常识直接略过*

## Android启程

一些介绍就不废话了。

### Android 系统架构

1. Linux 内核层

   主体是驱动部分

2. 系统运行库

   为Android提供支持的库，AndroidRuntime等

3. 应用架构

   Android Framework，系统提供的API

4. 应用层

![Android 架构](./assets/FirstCode/Android-Framework.svg.png)



### Android 应用开发特色

这些是应用开发主要接触的内容，大部分是需要精通的，少部分至少有些了解

1. 四大组建

   Activity 所看到的页面

   Service 后台运行的服务

   Broadcast 广播

   ContentProvider 内容提供者，为其他应用提供访问本应用数据的路径

2. 丰富的系统控件

   系统本身提供大量的API以供第三方应用使用

3. SQLite数据库

   轻量级数据库，适合移动端使用

4. 多媒体

   支持音视频照相等等，一个综合个人智能终端

5. 定位

   基于网络，gsm，gps定位，系统API比较笨拙且因为不可说的原因不好用，大部分时候是使用第三方的诸如高德百度等sdk



### 开发环境

直接去 [Android中国官网](https://developer.android.google.cn/) 下载AndroidStudio即可

​	*ps: baidu到的可能是第三方网站，建议bing或者google*

安装的时候无脑下一步就行

### 新建项目 启动应用

略

### 项目结构

基于Gradle，代码在app\src下面，java中是java代码，res中是布局图片等资源文件。

现在新建的Activity都基于`androidx.appcompat.app.AppCompatActivity`类

** 之前版本的Support包中的内容都存于各个版本诸如v4 v7等，新版本全部都整合到了androidx下，虽然之前的还能用，建议使用新api**



## Activity （活动）

*与用户交互的界面,绝大部分显示出来的都依托于activity*

### 创建

使用Android Studio的话直接new-> activity -> Empty Activity就可以了(快捷键alt+insert 然后输入activity)

自己创建的话,新建类extends Activity,然后在AndroidManifest.xml中注册:

```XML
<activity android:name=".chap09.HttpURLConnectionActivity" />
```

*ps: Android 四大组建都需要在AndroidManifest.xml中注册,否则会报错*

##### 布局

最快速且易用的编辑显示界面的方法

同样用Android Studio可以直接创建,选layout就行.

自己创建: 在res/layout目录中新建xml文件.在Activity的onCreate中调用`setContentView(R.layout.activity_http_u_r_l_connection);`就可以

##### 使用菜单（已经不是太常用了）

在`res/menu`中新建xml资源，Activity中重写：

```java
@Override
public boolean onCreateOptionsMenu(Menu menu) {
    getMenuInflater().inflate(R.menu.main, menu);
    return super.onCreateOptionsMenu(menu);
}
```

监听menu点击事件重写`public boolean onOptionsItemSelected( MenuItem item) {`

##### 启动制定Activity

显式： `startActivity(new Intent(Context,targetActivity.class));`

隐式：

```java
Intent intent = new Intent();
intent.setAction(targetAction);
// intent.addCategory(targetCategory); // 可选
startActivity(intent);
```

*action和category 均是Activity在AndroidManifest中注册时可增加的属性*

##### 一些常用的系统应用打开方式

使用隐式调用可以打开符合Intent的应用界面，下面是一些常用的：

```
new Intent().setData(Uri.parse("http://www.baidu.com")); // 用默认浏览器打开网页
new Intent().setData(Uri.parse("tel:10086")); // 拨号

```

Uri 也是配置在AndroidManifest中，属性如下：

> android: scheme。 用于 指定 数据 的 协议 部分， 如上 例 中的 http 部分。 android: host。 用于 指定 数据 的 主机 名 部分， 如上 例 中的 www. baidu. com 部分。 
>
> android: port。 用于 指定 数据 的 端口 部分， 一般 紧随 在 主机 名 之后。 android: path。 用于 指定 主 机名 和 端口 之后 的 部分， 如一 段 网址 中 跟在 域名 之后 的 内容。 
>
> android: mimeType。 用于 指定 可以 处理 的 数据 类型， 允许 使用 通配符 的 方式 进行 指定。

##### 传递数据：

`new Intent(target).putExtra(,)`目标可以用`getIntent().getxxxx`获取

##### 打开Activity并从中得到返回数据

如调用拍照等，可以在目标Activity拍完后拿到照片路径。

`startActivityForResult(Intent,requestCode)`并且重写`onActivityResult`可以在其中拿到返回的数据

### 生命周期

就是Activity在各个阶段会自动回调的方法。在其中添加我们自己的逻辑以便被调用。

Android是使用一个不可被应用访问的栈来管理。

##### 一个Activity会有四种状态

1. 运行态 可以被用户操作
2. 暂停状态 可见不可被操作。比如有弹窗时
3. 停止状态 完全不可见。这个状态不稳定，完全由系统决定何时被回收
4. 销毁 彻底被从栈中移除

##### 整个周期会被回调的方法

`onCreate` 被创建时调用，进行初始化操作，绑定布局等

`onStart` 变为可见时触发

`onResume` 可操作时触发

`onPause` 从可操作变为不可操作但可见时触发

`onStop` 不可见时触发

`onDestory` 被销毁时触发

`onRestart` 由停止重新运行时触发

![生命周期](./assets/FirstCode/2_activity_life.png)



##### 保存临时数据

处于onStop状态可能会被系统回收掉，如果重新返回这个页面，因为被回收会从新走onCreate，为了保存临时数据如EditText中的内容，可以重写`onSaveInstanceState(Bundle)`使用Bundle保存，在onCreate的bundle对象中读取。

##### Activity 启动模式

在AndroidManifest.xml中设置activity的launchMode属性，设置Activity被启动时的行为。

1. standard 默认，每次被打开都是在同一个栈中一个新的Activity
2. singleTop 当处于栈顶时不会重新创建，不处于栈顶打开新实例
3. singleTask 处于栈顶不会重新创建，处于栈中则弹出上面的Activity并显示已经存在的实例
4. singleTask *每个应用会有自己的栈* 设置了singleTask的Activity会有一个单独的栈。同一个应用打开的是相同的实例。*经验证，不同应用依旧打开不同实例*



## UI

Android UI由一个个控件组成。*大部分常用的略*

### 自定义控件

`<include>`标签只是引入布局，而使用自定义控件还可以复用事件相应。

如下自定义一个LinearLayout：

```
public class TitleLayout extends LinearLayout {
    public TitleLayout(Context context) {
        this(context,null);
    }

    public TitleLayout(Context context, AttributeSet attrs) {
        this(context, attrs,0);
    }

    public TitleLayout(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        LayoutInflater.from(context).inflate(R.layout.title, this);
    }
}
```

`R.layout.title`指向一个LinearLayout布局文件，引用的时候直接引用这个类替代`<include/>`

###### 需要补充自定义属性如何使用

### 列表

##### ListView

ListView 只是使用的话很简单，引入布局，定义Adapter，但是数据量多的时候会有加载的内容或者事件与对应位置index/position对应不上以及性能较差问题。使用ViewHolder来解决。

###### ViewHolder：

自定义Adapter，重写如下：

```Java

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        Frui fruit = getItem(position);
        View view;
        ViewHolder viewHolder;
        if (convertView == null) {
            view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false);
            viewHolder = new ViewHolder();
            viewHolder.fruitImage = (ImageView) view.findViewById(R.id.fruit_ image);
            viewHolder.fruitName = (TextView) view.findViewById(R.id.fruit_ name);
            view.setTag(viewHolder); // 将 ViewHolder 存储 在 View 中 
        } else {
            view = convertView;
            viewHolder = (ViewHolder) view.getTag(); // 重新 获取 ViewHolder

        } viewHolder.fruitImage.setImageResource(fruit.getImageId());
        viewHolder.fruitName.setText(fruit.getName());
        return view;
    }

    class ViewHolder {
        ImageView fruitImage;
        TextView fruitName;
    }
```

##### RecyclerView

更推荐使用RecyclerView来代替ListView，可配置性更高，并且默认使用ViewHolder，不用再自己考虑服复用问题。

1. 引入dependencies `androidx.recyclerview:recyclerview`

2. 引入布局

3. 自定义Adapter

   ```Java
   public class FruitAdapter extends RecyclerView.Adapter<FruitAdapter.ViewHolder> {
   
       private Context mContext;
       private List<Fruit> mFruitList;
   
       public FruitAdapter(List<Fruit> mFruitList) {
           this.mFruitList = mFruitList;
       }
   
       @NonNull
       @Override
       public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
           if (mContext == null) mContext = parent.getContext();
           View view = LayoutInflater.from(mContext).inflate(R.layout.fruit_item,parent,false);
           final ViewHolder holder = new ViewHolder(view);
           holder.cardView.setOnClickListener(new View.OnClickListener() {
               @Override
               public void onClick(View v) {
                   int position = holder.getAdapterPosition();
                   Fruit fruit = mFruitList.get(position);
                   Intent intent = new Intent(mContext, FruitActivity.class);
                   intent.putExtra(FruitActivity.FRUIT_NAME, fruit.getName());
                   intent.putExtra(FruitActivity.FRUIT_IMAGE_ID, fruit.getImageId());
                   mContext.startActivity(intent);
               }
           });
           return holder;
       }
   
       @Override
       public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
           Fruit fruit = mFruitList.get(position);
           holder.fruitName.setText(fruit.getName());
           Glide.with(mContext).load(fruit.getImageId()).into(holder.fruitImage);
       }
   
       @Override
       public int getItemCount() {
           return mFruitList.size();
       }
   
       public class ViewHolder extends RecyclerView.ViewHolder {
           CardView cardView;
           ImageView fruitImage;
           TextView fruitName;
           public ViewHolder(@NonNull View itemView) {
               super(itemView);
               cardView = (CardView) itemView;
               fruitImage = cardView.findViewById(R.id.fruit_image);
               fruitName = cardView.findViewById(R.id.fruit_name);
   
           }
       }
   }
   ```

4. 使用

   ```Java
   RecyclerView recyclerView = findViewById(R.id.recycler_view);
   GridLayoutManager layoutManager = new GridLayoutManager(this,3);
   recyclerView.setLayoutManager(layoutManager);
   adapter = new FruitAdapter(mFruitList);
   recyclerView.setAdapter(adapter);
   ```

   LayoutManager 常用有：
   
   * LinearLayoutManager 线性布局。类似默认的LinearLayout一条条添加，并且设置oritation可以实现横向列表
   * GridLayoutManager 网格布局，类似GridLayout
   * StaggeredGridLayoutManager 流式布局，类似GridLayoutManager只是每个条目高度不是固定的，而是适配条目本身大小。





## 碎片 Fragment

一开始是为了更容易适配平板,不过因为其灵活性,大部分应用都是把UI写在Fragment里面了.

### 使用方式

> (1) 创建待添加的碎片实例。
> (2) 获取FragmentManager,在活动中可以直接通过调用getSupportFragmentManager()方法
> 得到。
> (3) 开启一个事务,通过调用beginTransaction()方法开启。
> (4) 向容器内添加或替换碎片,一般使用replace()方法实现,需要传入容器的id和待添加
> 的碎片实例。
> (5) 提交事务,调用commit()方法来完成。

可以直接在布局中用<fragment android:name="target fragment class"/>

动态更换fragment:

```Java
private void replaceFragment(Fragment fragment) {
	FragmentManager fragmentManager = getSupportFragmentManager();
	FragmentTransaction transaction = fragmentManager.beginTransaction();
	transaction.replace(R.id.right_layout, fragment);
	transaction.commit();
}
```

*ps: 使用support包使代码适配范围更广,非support版本是从4.x开始的,不过现在影响不大,不过有的库是用的support使用时注意更改即可,其他support api同理*

### 用栈管理

```Java
private void replaceFragment(Fragment fragment) {
	FragmentManager fragmentManager = getSupportFragmentManager();
	FragmentTransaction transaction = fragmentManager.beginTransaction();
	transaction.replace(R.id.right_layout, fragment);
	transaction.addToBackStack(null);
	transaction.commit();
}
```

效果就是按back会先回退到上一个Fragment

### Fragment之间通信

```Java
RightFragment rightFragment = (RightFragment) getSupportFragmentManager()
.findFragmentById(R.id.right_fragment);
```

也可以用getActivity()获取Activity,然后用它中介

### 生命周期

1. 运行态 同Activity
2. 暂停态 同Activity,有弹窗时.
3. 停止态 
   * Activity处于停止态时
   * FragmentTransaction 调用remove,replce之前被addToBackStack,调用之后会停止,此时可能会被回收
4. 销毁
   * Activity被销毁
   * 没有被addToBackStack而被remove,replace

![Fragment 生命周期](./assets/FirstCode/4_fragment_lifecycle.png)



## Broadcast 广播

不同应用之间通信方式的一种.使用简单,效率一般.当系统中的任务太多,机器性能较差时可能会很晚才接到.

* 标准广播 发出来就不管了,不同应用接到的次序不定
* 有序广播 根据优先级依次接到,可以在高优先级的应用中拦截

### 接收

* 动态注册

  ```Java
  intentFilter = new IntentFilter();
  intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
  networkChangeReceiver = new NetworkChangeReceiver();
  registerReceiver(networkChangeReceiver, intentFilter);
  
  
  class NetworkChangeReceiver extends BroadcastReceiver {
      @Override
      public void onReceive(Context context, Intent intent) {
      Toast.makeText(context, "network changes", Toast.LENGTH_SHORT).show();
      }
  }
  ```

  regist部分走完之后receiver才能接到

* 静态注册

  ```XML
  <receiver
      android:name=".BootCompleteReceiver"
      android:enabled="true"
      android:exported="true">
          <intent-filter>
          	<action android:name="android.intent.action.BOOT_COMPLETED" />
          </intent-filter>
  </receiver>
  
  ```

  然后类`extends BroadcastReceiver`即可.这样只要应用安装后启动过一次,就一直都能接到广播

  *ps1: 因为广播滥用,很多ODM长都对第三方应用广播进行了限制,这个可能接不到就不在这里讨论了*

  *ps2: 大部分时候是需要接收系统广播的,很多都需要相应权限,这个注意*

### 发送

* 标准广播`sendBroadcast(Context,Intent(TargetAction))`

* 有序广播`sendOrderedBroadcast()` receiver注册时增加`<intent-filter android:priority="100">`就可以决定接收顺序了,然后onReceive里面调用abortBroadcast可以中断广播不让后面的接收器接到



### 本地广播

仅本应用内部可以接发,其他应用接不到的广播.比全局广播效率要高一些,不过因为还是异步,尽量少用.

相较于全局广播,只是使用时调用下LocalBroadcastManager

```Java
// 获取manager
localBroadcastManager = LocalBroadcastManager.getInstance(Context); 
// 发送
localBroadcastManager.sendBroadcast(intent);
// 注册receiver
localBroadcastManager.registerReceiver(localReceiver, intentFilter);
```

**本地广播接收器无法静态注册**



## 持久化 数据存储

主要是文件存储,SharedPreference和数据库.

### 文件存储

可以储存简单的文本,二进制数据或者大文件.

通过Context.openFileOutput("name",MODE)来写入,获取文件流然后用Java的文件访问方式处理,Mode分为MODE_PRIVATE(默认方式,写入会覆盖源文件)和MODE_APPEND(如果源文件存在则会在后面追加)

通过Context.openFileInput("name")来读取

文件会默认保存在`/data/data/包名/files`中

### SharedPreference

主要用来存储配置信息,设置项等.

