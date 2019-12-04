### 手册目录

> 已设置锚点，点击下方文字，即可跳转到对应段落。
>
> 点击锚点后，摁返回键，即可回退到此处。

- [01.创建项目文件](#01)


- [02.九宫格界面设计](#02)


- [03.自定义按钮样式](#03)


- [04.仿“水纹效果”](#04)


- [05.隐藏标题栏](#05)


- [06.半透明状态栏](#06)


- [07.跳转页面、加Q、加群](#07)


- [08.跳转到浏览器打开网页](#08)


- [09.优化网页滑动效果](#09)


- [10.修复播放视频黑屏](#10)


- [11.修复图片大小异常](#11)


- [12.移除网页右侧滚动条](#12)


- [13.将网页提示框转为文字提示](#13)


- [14.封装WebView](#14)


- [15.修复页面加载白屏问题](#15)


- [16.调用下载器/浏览器下载文件](#16)


- [17.签名、打包](#17)


- [18.彩蛋篇](#18)

---

### 实战开发

### 一、<a id="01">创建项目文件</a>

##### 开发工具

本手册使用AIDE作为开发、打包工具。AIDE的代码编辑体验较差，故使用MT作为代码编辑器。

这两个软件都可以在应用商店下载。如果你加了小北IT付费群的话，灵动版群文件顶部搜索框，了解一下。

##### 配置AIDE

AIDE不是拿起来就能用的。从软件右上角的菜单项进入“设置”，找到“构建&运行”：

![](https://huidu666.github.io/huidu/kaiyuan/img/001.png)

![](https://huidu666.github.io/huidu/kaiyuan/img/002.png)

勾选底部的Dexer优化。然后配置SDK（android.jar）路径，如：

```
/storage/emulated/0/AppProjects/android.jar
```

SDK可在网上自行下载，也可在灵动版群文件搜索获取。这里不单独提供。

##### 创建项目

在没有打开任何项目的情况下，可以看到下边这个界面：

![](https://huidu666.github.io/huidu/kaiyuan/img/003.png)

进入AppProjects文件夹（/storage/emulated/0/AppProjects/是AIDE的默认项目路径）。

点击新建项目：

![](https://huidu666.github.io/huidu/kaiyuan/img/004.png)

选择“新建Android App”项目：

![](https://huidu666.github.io/huidu/kaiyuan/img/005.png)

以灰度为例：

![](https://huidu666.github.io/huidu/kaiyuan/img/006.png)

文件名称、相关代码可参阅灰度源码文件，这里不做具体展示。

不可能五百多行代码一行一行得讲，我不讲Java基础，我什么也不懂，没资格讲基础知识。

---

### 二、<a id="02">九宫格界面设计</a>

#### 设计分析

辟个谣，灰度九宫格用的是“表格布局”，不是“网格布局”。表格布局理解起来相对较难，但实际使用很方便。

灰度主界面，以及"灰度:D"二级界面，均使用了九宫格布局，只是对应的按钮（Button）ID不一样、按钮文字不一样、点击事件不一样。这三个“不一样”统统不涉及界面布局。一样的地方，索性写成“按钮样式文件”，这个后边会讲到。

#### main.xml

以下是main.xml（灰度主界面的布局文件）的代码示例图：

![](https://huidu666.github.io/huidu/kaiyuan/img/007.png)

#### 线性布局

最底层使用“线性布局”（LinearLayout）。示例代码如下：

```xml
<LinearLayout
	xmlns:android="http://schemas.android.com/apk/res/android"
     //对齐方式设为水平居中
	android:orientation="horizontal"
	//宽度高度设为最大值就好啦
	android:layout_width="match_parent"
	android:layout_height="match_parent"
	//背景颜色
	android:background="#282828"
	//允许调整按钮宽度
	android:clipToPadding="true"
     //给系统状态栏留出空间
	android:fitsSystemWindows="true" >
```

#### 表格布局

在线性布局内创建表格布局（TableLayout），并添加代码：

```xml
android:stretchColumns="*"
```

*是全部的意思，这个不难理解。那这行代码有什么用呢？

众所周知，市面上各家的手机长相并非一致，有宽屏的有窄屏的。常见的屏幕宽度有360dp（大小的单位）的，也392dp的。那你直接把按钮写成死数（固定值），好使吗？

这时候就要用到权重的思想了，让按钮根据比例来调整自身宽度，即“屏幕适配”。

android:stretchColumns="*"的作用就是该表格布局内的所有控件都可以“调整”自身宽度。

注：线性布局里的android:clipToPadding="true"决定了是否允许调整控件大小。所以这二者是要配合使用的。

还有，表格布局的对齐方式是“垂直居中”，即：

```XML
android:orientation="vertical"
```

#### 表格项及按钮

在表格布局内添加三个“表格项”（TableRow）（一个表格项用来生成一行，九宫格是三行的，所以是三个表格项）。每个表格项内添加三个按钮控件（Button）。

以灰度主界面第一个按钮为例，相关布局代码如下：

```xml
		<Button
			android:text="肥宅水"
			style="@style/btn"
			android:id="@+id/btn01"/>
```
很明显，text后边引号里的内容，就是按钮内显示的文字。

style="@style/btn"指该按钮“调用”了我自定义的btn.xml样式文件，这样可以省去大量重复代码。

ID不难理解吧，类似于学生的学号，就算重名也能进行区分。

#### huidu.xml

灰度:D二级界面和主界面的布局大同小异，不再重复说明。

#### browser.xml

这个是用来访问网页的，只需要一个Webview控件。代码我就不贴了，自己去看源码文件就是了。

---

### 三、<a id="03">自定义按钮样式</a>

前边我们讲了，style="@style/btn"是自定义按钮样式。具体实现其实也很简单：

values文件夹内新建btn.xml文件，示例代码如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <style name="btn">
	<item name="android:background">@drawable/btn</item>
	<item name="android:textSize">14sp</item>
	<item name="android:textColor">#B6B6B6</item>
	<item name="android:layout_weight">1.0</item>
	<item name="android:clickable">true</item>
	<item name="android:layout_height">132dp</item>
  </style>
</resources>
```

为什么背景颜色也要去“调用”呢？为了实现仿“水纹效果”。下边讲会到这个。

---

### 四、<a id="04">仿“水纹效果”</a>

drawable文件夹内新建btn.xml文件，示例代码如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="false">
        <shape>
            <solid android:color="#282828" />
        </shape>
    </item>
    <item android:state_pressed="true">
        <shape>
            <solid android:color="#1d1d1d"/>
        </shape>
    </item>
</selector>
```
代码很简单，一个是未点击时的背景颜色，一个是被点击时的背景颜色。

color后边引号里的颜色值可以自己替换。

---

### 五、<a id="05">隐藏标题栏</a>

AndroidManifest.xml文件，主题改为：

```xml
android:theme="@android:style/Theme.NoTitleBar"
```

style.xml文件，代码改为：

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="AppTheme" parent="@android:style/Theme.Holo.Light">
	</style>
</resources>
```

---

### 六、<a id="06">半透明状态栏</a>

灰度有三个Java类，分别是MainActivity、HuiduActivity、WebActivity。

这三个类都各自创建了新的界面，所以都会有一行这样的代码

```xml
setContentView(R.layout.xxx);
```

注：xxx是对应XML布局的文件名。

在这行代码上边插入以下三行代码，即可让状态栏和底部导航栏实现半透明：

```java
requestWindowFeature(Window.FEATURE_NO_TITLE);
		getWindow().addFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS);
		getWindow().addFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_NAVIGATION);
```

---

### 七、<a id="07">跳转页面、加Q、加群</a>

控件ID、QQ号、QQ群key什么的，自己改。

QQ群KEY获取地址：https://qun.qq.com/join.html

使用方法：长摁复制上边的链接，粘贴到手机QQ或电脑浏览器。申请QQ群的KEY。

#### 页面跳转

以点击按钮为例，以下代码可实现对应的事件，写在   setContentView(R.layout.xxx);   的下边：

```java
Button 按钮ID = findViewById(R.id.按钮ID);
    按钮ID.setOnClickListener(new View.OnClickListener() {
	@Override
	public void onClick(View v){
	Intent intent = new Intent(当前类的名称.this, 跳转后类的名称.class);
	startActivity(intent);
	}});
```

#### 加Q代码

以点击按钮为例，以下代码可实现对应的事件，写在   setContentView(R.layout.xxx);   的下边：

```java
Button 按钮ID= findViewById(R.id.按钮ID);
    按钮ID.setOnClickListener(new View.OnClickListener() {
	@Override
	public void onClick(View v){
        String url="mqqwpa://im/chat?chat_type=wpa&uin=QQ号码";
        startActivity(new Intent(Intent.ACTION_VIEW, Uri.parse(url))); 
	}});
```

#### 加群代码

以点击按钮为例，以下代码可实现对应的事件，写在   setContentView(R.layout.xxx);   的下边：

```java
Button 按钮ID= findViewById(R.id.按钮ID);
	按钮ID.setOnClickListener(new View.OnClickListener(){
	@Override
	public void onClick(View p1){
        joinQQGroup("替换为你申请的QQ群的KEY");
		}});
```

在protected void onCreate(Bundle savedInstanceState){代码块}外边，添加以下代码：

```java
public boolean joinQQGroup(String key){
  Intent intent = new Intent();
  intent.setData(Uri.parse("mqqopensdkapi://bizAgent/qm/qr?url=http%3A%2F%2Fqm.qq.com%2Fcgi-bin%2Fqm%2Fqr%3Ffrom%3Dapp%26p%3Dandroid%26k%3D" + key));
  try{
    startActivity(intent);
	return true;
  }
   catch (Exception e){
	return false;
   }}
```

---

### 八、<a id="08">跳转到浏览器打开网页</a>

以点击按钮为例，以下代码可实现对应的事件，写在   setContentView(R.layout.xxx);   的下边：

```java
Button 按钮ID= findViewById(R.id.按钮ID);
按钮ID.setOnClickListener(new View.OnClickListener() {
	@Override
	public void onClick(View v){
		Uri uri = Uri.parse("网址");
		Intent intent = new Intent(Intent.ACTION_VIEW, uri);
		startActivity(intent);
		}});
```
如果设置了默认浏览器，则直接使用你设置的默认浏览器来打开网页。

---

### 九、<a id="09">优化网页滑动效果</a>

继续辟谣。网页滑动流畅问题，在xml布局文件里设置什么软件加速、硬件加速是没有用的。

正确的做法是在java里开启硬件加速，关键代码如下：

```java
public class WebActivity extends Activity{
	private WebView wv;
    @Override
    public void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
        setContentView(R.layout.browser);
        //启用硬件加速
        wv.setLayerType(View.LAYER_TYPE_HARDWARE, null);
    }
```

好吧，贴这么多代码，就是为了让你看清楚最后这行代码放在了哪。

---

#### 十、<a id="10">修复播放视频黑屏</a>

看到上边第六行代码了吧？setContentView(R.layout.browser);在它上边添加这三行代码：

```java
getWindow().setFlags(
		WindowManager.LayoutParams.FLAG_HARDWARE_ACCELERATED,
		WindowManager.LayoutParams.FLAG_HARDWARE_ACCELERATED);
```
这样就启用硬件渲染了，解决播放视频时黑屏的问题。

---

### 十一、<a id="11">修复图片大小异常</a>

紧挨着上边的启用硬件加速的代码（wv.setLayerType(View.LAYER_TYPE_HARDWARE, null);）的附近。

其他的一些代码我也就一并贴上吧：

```java
//移除网页中的滚动条
wv.setHorizontalScrollBarEnabled(false);
wv.setVerticalScrollBarEnabled(false);
// 设置隐藏缩放工具
wv.getSettings().setBuiltInZoomControls(false);
//允许扩大比例的缩放 
wv.getSettings().setUseWideViewPort(true);
//支持javascript 
wv.getSettings().setJavaScriptEnabled(true);
//自适应屏幕  
wv.getSettings().setLayoutAlgorithm(WebSettings.LayoutAlgorithm.SINGLE_COLUMN);
wv.getSettings().setLoadWithOverviewMode(true);	
// 设置可以支持缩放 
wv.getSettings().setSupportZoom(true);
```

---

### 十二、<a id="12">移除网页右侧滚动条</a>

看上边的代码。

为什么要移除掉滚动条呢？我看它不顺眼。。

当然，你可以尝试重写它，自定义一个你喜欢的风格的滚动条。

---

### 十三、<a id="13">将网页提示框转为文字提示</a>

紧接着第十一条，粘贴以下代码：

```java
wv.setWebChromeClient(new WebChromeClient(){
	@Override
	public boolean onJsAlert(WebView view, String url, String message, JsResult result){
	Log.i("WebActivity", "onJsAlert url=" + url + ";message=" + message);
	Toast.makeText(getApplicationContext(), message, Toast.LENGTH_SHORT).show();
	result.confirm();
	return true;
	}
```

可以将提示框转换为时间较短的文字提示。如果需要时间较长的话，把Toast.LENGTH_SHORT改为Toast.LENGTH_LONG即可。

---

### 十四、<a id="14">封装WebView</a>

封装的好处是，可以直接从外部传入Url。否则只能一个类里写一条链接，造成大量重复代码。多么的可怕！

所以，不要听网上那些人说的，webview.loadurl("网址");真的是害人不浅。。

#### 封装WebView

添加代码private String webViewUrl;，具体位置如下：

```java
public class WebActivity extends Activity
{
	private String webViewUrl;
    //省略其他代码
}
```

添加以下代码：

```java
private void getData()
	{
        webViewUrl = getIntent().getStringExtra("webview_url");
    }

private void initViews()
{
	wv = findViewById(R.id.webView);
	wv.loadUrl(webViewUrl);
}

private void loadData()
{
	wv.loadUrl(webViewUrl);
}

public static void skip(Activity MainActivity, String webviewUrl)
{
    Intent intent = new Intent(MainActivity, WebActivity.class);
    intent.putExtra("webview_url", webviewUrl);
    MainActivity.startActivity(intent);}
```

不清楚放哪的话，我教你咋整。该类的最后一个字符是}，对吧？放到它前边。。

上述第17-21行，实现了接收MainActivity类中传入的Url。如果其它类也需要的话，复制这段代码、更改skip和MainActivity即可。如：

```java
； public static void skip2(Activity HuiduActivity, String webviewUrl)
	{
        Intent intent = new Intent(HuiduActivity, WebActivity.class);
        intent.putExtra("webview_url", webviewUrl);
        HuiduActivity.startActivity(intent);}
```

#### 传入Url

以灰度界面的“检测更新”按钮为例：

```java
Button btn19 = findViewById(R.id.btn19);
    btn19.setOnClickListener(new View.OnClickListener() {
	@Override
	public void onClick(View v){
        WebActivity.skip2(HuiduActivity.this, "https://huidu666.github.io/update.html");
    }});
```

其他按钮的代码以此类推，注意控件ID不要写错。

---

### 十五、<a id="15">修复页面加载白屏问题</a>

灰度的资源全都托管到Github了，加载速度的确较慢。但即便挂了梯子，切换页面时仍然会闪白屏。

咋修复嘞？把这两行代码加到上边的第十一条里边。

```java
wv.setBackgroundColor(0);
wv.getBackground().setAlpha(2);
```

这两行代码，大概意思就是说，背景颜色设置为透明，并且透明度为2。

---

### 十六、<a id="16">调用下载器/浏览器下载文件</a>

万事大吉？下载个文件试试。

没反应吧。嗯，还得再加点代码。还是挨着上边第十一条。

```java
wv.setDownloadListener(new DownloadListener() {
	@Override
	public void onDownloadStart(String url, String userAgent, String contentDisposition, String mimetype, long contentLength){
	Intent i = new Intent(Intent.ACTION_VIEW);
	i.setData(Uri.parse(url));
	startActivity(i);
	}});
```

推荐搭配使用ADM（应用商店下载，或者灵动版群文件搜索获取）。

---

### 十七、<a id="17">签名、打包</a>

千万不要急着打包。AndroidManifest.xml文件，添加联网权限没？注册Activity没？

详见源码文件吧。补贴代码了。

以上所有的操作，都是在src文件夹进行的。最后，找到和它同级的文件build.gradle。进去改版本号。

然后，签名打包测试。软件就算搞好了。

---

### 十八、<a id="18">彩蛋篇</a>

[网站开发手册](https://huidu666.github.io/website.html)

---

### 写在最后

禁止将此软件用于直接或间接的获利行为。

二改“灰度”时，必须注明软件出自“小北IT”，必须保留肥宅水、企鹅号、悬浮标记这三个按钮，且相关联的内容不得进行替换。禁止在软件内添加任何广告。

可通过下方联系方式找到我，加入小北IT付费群：

- 联系我：


企鹅号：1653131174

微信号：liyizhuang0608

- 灰度用户群：

群号：419813381

- 官方反馈邮箱：

liyizhuang@xiaobeiit.com

liyizhuang@xiaobeiit.cn

注：此手册谢绝转载。

2019.12.04 00:30

---

<center>小壮壮/Write</center>
