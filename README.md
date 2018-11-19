#### 本文属于个人平时项目开发过程遇到的一些问题，记录下来并总结解决方案，希望能帮到大家解决问题，有些问题的解决方案是在StackoverFlow上找到的，建议大家遇到问题多去上面找，基本上都能找到解决方案的。各位童鞋也可以把自己平常遇到的一些疑难杂症提Pr上来，我会合并上去。

（1）**将Eclipse项目导入到Android studio 中 很多点9图出现问题解决方法：**

在build.gradle里添加以下两句：
```
aaptOptions.cruncherEnabled = false     
aaptOptions.useNewCruncher = false
```
用来关闭Android Studio的PNG合法性检查的，直接不让它检查。

（2）**Android Studio 错误: 非法字符: '\ufeff' 解决方案|错误: 需要class, interface或enum **

- **原因：**
Eclipse可以智能的把UTF-8+BOM文件[转为](http://www.cfanz.cn/index.php?c=search&key=%E8%BD%AC%E4%B8%BA)普通的UTF-8文件，Android Studio还没有这个功能，所以使用Android Studio编译UTF-8+BOM[编码](http://www.cfanz.cn/index.php?c=search&key=%E7%BC%96%E7%A0%81)的文件[时会](http://www.cfanz.cn/index.php?c=search&key=%E6%97%B6%E4%BC%9A)出现” [非法](http://www.cfanz.cn/index.php?c=search&key=%E9%9D%9E%E6%B3%95)字符: '\ufeff' “[之类](http://www.cfanz.cn/index.php?c=search&key=%E4%B9%8B%E7%B1%BB)的错误

- **解决方法：**
[手动](http://www.cfanz.cn/index.php?c=search&key=%E6%89%8B%E5%8A%A8)将UTF-8+BOM编码的文件转为普通的UTF-8文件。**
**用EdItPlus打开.java文件[依次](http://www.cfanz.cn/index.php?c=search&key=%E4%BE%9D%E6%AC%A1)：文档》文本编辑》转换文本编码》选择UTF-8编码即可

（3）**将项目导入到AS中出现以下问题：**

```
Error:Execution failed for task ':app:transformResourcesWithMergeJavaResForDebug'. > com.android.bui
```
- **解决方法：**
在build.grade中添加以下代码：
```
android{
packagingOptions {
exclude 'META-INF/DEPENDENCIES.txt'
exclude 'META-INF/NOTICE'
exclude 'META-INF/NOTICE.txt'
exclude 'META-INF/LICENSE'
exclude 'META-INF/LICENSE.txt'
          }
}
```

（4）**未知错误**
```
Error:Timeout waiting to lock cp_proj class cache for build file '/Users/Mr.xiao/Desktop/AndroidShopNC2014MoblieNew/androidShopNC2014Moblie/build.gradle' 
(/Users/Mr.xiao/.gradle/caches/2.10/scripts/build_3cyr7hzjurcc62ge3ixidshos/cp_proj).
It is currently in use by another Gradle instance.
Owner PID: unknown
Our PID: 1412
Owner Operation: unknown
Our operation: Initialize cache
Lock file: /Users/Mr.xiao/.gradle/caches/2.10/scripts/build_3cyr7hzjurcc62ge3ixidshos/cp_proj/cache.properties.lock
```
- **解决方案**
以上是错误提示。
解决的思路很简单只需要把**cache.properties.lock文件**删除了就可以了。当时我们删除的时候会被占用这时候需要进入任务管理器结束关于java的进程就行比如 java 的jdk 删除后重启让java jdk启动  启动Android Studio就能启动APK了。

（5）**修改了Android项目的最小SDK版本之后出现很多stysle文件找不到**
- **解决方案**
```
compileSdkVersion 23
buildToolsVersion "23.0.3"
defaultConfig {
applicationId "net.mmloo2014.android"
minSdkVersion 14
targetSdkVersion 23
}
```

compileSdkVersion 是多少版本的

那么compile 'com.android.support:appcompat-v7:23.2.1’ 就是啥版本的。

（6）**Android studio 编译问题：finished with non-zero exit value 2**
- **问题：**

```
Error:Execution failed for task ':androidShopNC2014Moblie:transformClassesWithDexForDebug'.
>
com.android.build.api.transform.TransformException: 
com.android.ide.common.process.ProcessException: 
java.util.concurrent.ExecutionException: 
com.android.ide.common.process.ProcessException: 
org.gradle.process.internal.ExecException: 
Process 'command '/Library/Java/JavaVirtualMachines/jdk1.7.0_79.jdk/Contents/Home/bin/java'' finished with non-zero exit value 2
```
- **解决方案**
这个错误在app的build.gradle里面添加下面这句就好了。

```
android { 
defaultConfig { 
multiDexEnabled true
      }
}
```
（7）**Android studio 编译问题：finished with non-zero exit value 1（由于导入的依赖出现重复造成的）**
- **问题：**

```
Error:Execution failed for task ':app:transformClassesWithDexForDebug'.

> com.[Android](http://lib.csdn.net/base/15).build.api.transform.TransformException: com.android.ide.common.process.ProcessException: org.gradle.process.internal.ExecException: Process 'command 'F:\Program Files (x86)\[Java](http://lib.csdn.net/base/17)\jdk1.8.0_31\bin\java.exe'' finished with non-zero exit value 1
```
- **解决方案**
这个是因为依赖包重复了 （像v4和nineoldandroids），app中实现了对easeUI的依赖，但是app和easeUI都添加了对这个包的依赖。所以就报这个错误，修改之后再报，就clean，rebuild一下。

（8）**问题**
```
Error:Execution failed for task 
':app:transformClassesWithJarMergingForDebug'.> 
com.android.build.api.transform.TransformException: 
java.util.zip.ZipException:
 duplicate entry: org/apache/http/ConnectionClosedException.class
```
- **解决方案**
这个是在我们启动的时候报错的，而不是在编译的时候，原因是这样的，报这个错是因为有2个库中存在相同的类。大家可以看到stackoverflow上有人也提了这样的问题。只需要删除其中的一个就可以解决了。

（9）**添加第三方依赖出现的问题**

```
Error:Execution failed for task ':app:processDebugManifest'.
> 
Manifest merger failed :
 uses-sdk:minSdkVersion 14 cannot be smaller than version 19 declared in library [com.github.meikoz:basic:2.0.3] 
/AndroidStudioCode/EnjoyLife/app/build/intermediates/exploded-aar/
com.github.meikoz/basic/2.0.3/AndroidManifest.xml
Suggestion: use tools:overrideLibrary="com.android.core" to force usage
```
- **错误原因**
出现这个错误的原因是我引入的第三方库最低支持版本高于我的项目的最低支持版本，异常中的信息显示：我的项目的最低支持版本为14，而第三方库的最低支持版本为19，所以抛出了这个异常。

- **解决方案**
在AndroidManifest.xml文件中标签中添加

```
<uses-sdk tools:overrideLibrary="xxx.xxx.xxx"/>

```
其中的xxx.xxx.xxx为第三方库包名，如果存在多个库有此异常，则用逗号分割它们，例如：
```
<uses-sdk tools:overrideLibrary="xxx.xxx.aaa, xxx.xxx.bbb"/>
```
这样做是为了项目中的AndroidManifest.xml和第三方库的AndroidManifest.xml合并时可以忽略最低版本限制。

（10）**Android studio 编译问题：finished with non-zero exit value 1（由于buildtools版本太高造成的）**

- **错误**

```
Error:Execution failed for task ':app:transformClassesWithDexForDebug'.
> com.android.ide.common.process.ProcessException: 
org.gradle.process.internal.ExecException: 
Process 'command '/Library/Java/JavaVirtualMachines/jdk1.7.0_79.jdk/Contents/Home/bin/java'' finished with non-zero exit value 1
```
- **错误原因**
buildToolsVersion版本太高，我原来的 buildToolsVersion "24.0.0” 需要jdk1.8，而我的是jdk1.7，所以一直报这个错，刚开始以为是v4包和V7包冲突，因为之前遇到这样的问题，而这次删除V4包之后依然报这个错，上stackoverflow搜了一下，把buildTools版本降下来就好了。

- **解决方案**

```
android {    
compileSdkVersion 23    
buildToolsVersion "23.0.3"  
}
```

（11）**Android studio 编译问题：Gradle DSL not found 'android()'**
- **问题**

![clipboard.png](http://upload-images.jianshu.io/upload_images/1956769-db875d7c40e06441.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- **解决方案**
- 配置build.gradle:

```
buildscript {
repositories {   
jcenter()
}
    
dependencies {   
classpath 'com.android.tools.build:gradle:2.1.2'
   }
}

allprojects {  
repositories {    
jcenter()
   }
}
buildscript {
repositories {      
jcenter()
}
    
dependencies {
classpath 'com.android.tools.build:gradle:2.1.2'
    }
}


allprojects {
repositories {
jcenter()
    }
}
```
- 配置app/build.gradle:

```
apply plugin: 'com.android.application'android { 
compileSdkVersion 23
 buildToolsVersion '23.0.3' 
defaultConfig { 
minSdkVersion 9 
targetSdkVersion 23 
versionCode 1 
versionName '1.0' 
             }
}
dependencies { 
compile 'com.android.support:appcompat-v7:23.2.1'
}
```

**最后再同步一下sync即可。**

（12）**Android studio 编译问题：Gradle DSL not found 'android()'**

- **问题描述**

```
Error:(51, 52) 错误: -source 1.6 中不支持 diamond 运算符
(请使用 -source 7 或更高版本以启用 diamond 运算符)

```
- **解决方案**
- **方案一**

![将标红处设置为1.7.png](http://upload-images.jianshu.io/upload_images/1956769-6d0b6c2b6485221b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![修改soure为1.7.png](http://upload-images.jianshu.io/upload_images/1956769-cb3e8f40d6c34ffc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- **方案二**
在build gradle中进行配置如下代码：
```
android {
 compileOptions {
 sourceCompatibility JavaVersion.VERSION_1_7
 targetCompatibility JavaVersion.VERSION_1_7
 }
}
```
**最后同步一下即可**

（13）**Glide使用问题：使用Glide加载圆角图片，第一次显示占位图**
- **问题描述**
最近在项目中使用Glide加载圆形图片，并且设置placehloder和error两个占位图，运行发现，第一次加载图片只显示占位图，需要第二次进入的时候才会正常显示。
如果你刚好使用了这个圆形[Imageview库](https://github.com/hdodenhof/CircleImageView)或者其他的一些自定义的圆形Imageview，而你又刚好设置了占位的话，那么，你就会遇到第一个问题。如何解决呢？

- **方案一**
不设置占位图

- **方案二**
使用Glide的Transformation API自定义圆形Bitmap的转换

```
   /**
     * Glide圆形图片处理
     */
    static class CircleTransform extends BitmapTransformation {
        public CircleTransform(Context context) {
            super(context);
        }

        @Override
        protected Bitmap transform(BitmapPool pool, Bitmap toTransform, int outWidth, int outHeight) {
            return circleCrop(pool, toTransform);
        }

        private static Bitmap circleCrop(BitmapPool pool, Bitmap source) {
            if (source == null) return null;

            int size = Math.min(source.getWidth(), source.getHeight());
            int x = (source.getWidth() - size) / 2;
            int y = (source.getHeight() - size) / 2;

            Bitmap squared = Bitmap.createBitmap(source, x, y, size, size);

            Bitmap result = pool.get(size, size, Bitmap.Config.RGB_565);
            if (result == null) {
                result = Bitmap.createBitmap(size, size, Bitmap.Config.ARGB_8888);
            }

            Canvas canvas = new Canvas(result);
            Paint paint = new Paint();
            paint.setShader(new BitmapShader(squared, BitmapShader.TileMode.CLAMP, BitmapShader.TileMode.CLAMP));
            paint.setAntiAlias(true);
            float r = size / 2f;
            canvas.drawCircle(r, r, r, paint);
            return result;
        }

        @Override
        public String getId() {
            return getClass().getName();
        }
    }

```
**使用方法：**
```
 Glide.with(context).load(imageUrl).placeholder(placeholder).error(errorImage).transform(new CircleTransform(context)).into(imageView);
```
**方案三**
重写Glide的图片加载监听方法，具体如下：

```
Glide.with(mContext) 
.load(url) 
.placeholder(R.drawable.loading_drawable) 
.into(new SimpleTarget<Bitmap>(width, height) {
 @Override public void onResourceReady(Bitmap bitmap, GlideAnimation anim) { 
// setImageBitmap(bitmap) on CircleImageView  
} 
});
```
**注意事项：**
该方法在listview上复用有问题的bug,如果在listview中加载CircleImageView，请不要使用该方法。

**方案四：不使用Glide的默认动画：**
```

Glide.with(mContext)
    .load(url) 
    .dontAnimate()
    .placeholder(R.drawable.loading_drawable)
    .into(circleImageview);
```

（14）**json数据解析问题：json串头部出现字符："\ufeff" 解决方法**
**异常信息**
```
org.json.JSONException: Value ﻿ of type java.lang.String cannot be converted to JSONObject
```
解析服务器返回 的json格式数据时，我们可能会发现，数据格式上是没有问题的，但是仔细对比会发现，在json串头部发现字符："\ufeff" 

**客户端解决方案：**
```
/** 
* 异常信息：org.json.JSONException: Value ﻿ of type java.lang.String cannot be converted to JSONObject 
* json串头部出现字符："\ufeff" 解决方法
* @param data 
* @return 
*/
public static final String removeBOM(String data) {    
if (TextUtils.isEmpty(data)) {        
         return data;    
}    
if (data.startsWith("\ufeff")) {        
        return data.substring(1);    
       } 
else {        
        return data;    
        }
}
```

**服务器端解决方案：**
将输出此json的php源码重新用editplus之类用utf-8无BOM的编码保存。不要用windows系统自带的记事本编辑php源码，这个BOM就是记事本这些windows自带的编辑器引入的。

（15）**Android studio编译问题：not found ndk()**
**问题**
```
Error:(15, 0) Gradle DSL method not found: 'ndk()' method-not-found-ndk
```
**解决方案**
出现该问题，可能是由于ndk配置在build.gradle配置文件中位置弄错导致的
```
apply plugin: 'com.android.application'
android {
    compileSdkVersion 23
    buildToolsVersion "23.0.2"

    defaultConfig {
        applicationId "com.guitarv.www.ndktest"
        minSdkVersion 17
        targetSdkVersion 23
        versionCode 1
        versionName "1.0"
        ndk {
            moduleName = "HelloJNI"
        }
        sourceSets.main {
            jni.srcDirs = []
            jniLibs.srcDir "src/main/libs"
        }
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}
```

（16）**Android studio导入其他的项目：UnsupportedMethodException**
**问题**
```
UnsupportedMethodException
        Unsupported method: AndroidProject.getPluginGeneration().
        The version of Gradle you connect to does not support that method.
        To resolve the problem you can change/upgrade the target version of Gradle you connect to.
        Alternatively, you can ignore this exception and read other information from the model.
```


![错误截图](http://upload-images.jianshu.io/upload_images/1956769-8da880f5e33514b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**解决方案**

将根目录中的build.gradle文件中的gradle版本号,出现错误之前，我的是1.3.0，修改成2.2.0之后重新编译一下就可以运行了。
```
dependencies {    
classpath 'com.android.tools.build:gradle:1.3.0'   
}
```
将这个版本号改成你其他项目能够运行成功的版本号即可

（17）**Android studio更新到2.1.1之后使用CollapsingToolbarLayout出现Error inflating class CollapsingToolbarLayout**
之前在项目中使用了CollapsingToolbarLayout，效果还是可以的，但是Android stuido更新到2.1.1版本之后出现Error inflating class CollapsingToolbarLayout 异常崩溃
**异常信息如下所示：**
```
com.test.android/com.test.android.ui.activity.RandomActivity}: android.view.InflateException: Binary XML file line #22: Error inflating class android.support.design.widget.CollapsingToolbarLayout
                                                                      at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2325)
                                                                      at android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:2387)
                                                                      at android.app.ActivityThread.access$800(ActivityThread.java:151)
                                                                      at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1303)
                                                                      at android.os.Handler.dispatchMessage(Handler.java:102)
                                                                      at android.os.Looper.loop(Looper.java:135)
                                                                      at android.app.ActivityThread.main(ActivityThread.java:5254)
                                                                      at java.lang.reflect.Method.invoke(Native Method)
                                                                      at java.lang.reflect.Method.invoke(Method.java:372)
                                                                      at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:903)
                                                                      at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:698)
                                                                   Caused by: android.view.InflateException: Binary XML file line #22: Error inflating class android.support.design.widget.CollapsingToolbarLayout
                                                                      at android.view.LayoutInflater.createView(LayoutInflater.java:633)
                                                                      at android.view.LayoutInflater.createViewFromTag(LayoutInflater.java:743)
                                                                      at android.view.LayoutInflater.rInflate(LayoutInflater.java:806)
                                                                      at android.view.LayoutInflater.rInflate(LayoutInflater.java:809)
                                                                      at android.view.LayoutInflater.rInflate(LayoutInflater.java:809)
                                                                      at android.view.LayoutInflater.inflate(LayoutInflater.java:504)
                                                                      at android.view.LayoutInflater.inflate(LayoutInflater.java:414)
                                                                      at android.view.LayoutInflater.inflate(LayoutInflater.java:365)
                                                                      at android.support.v7.app.AppCompatDelegateImplV7.setContentView(AppCompatDelegateImplV7.java:276)
                                                                      at android.support.v7.app.AppCompatActivity.setContentView(AppCompatActivity.java:136)
                                                                      at com.test.android.ui.activity.RefreshableActivity.onCreate(RefreshableActivity.java:31)
                                                                      at android.app.Activity.performCreate(Activity.java:5990)
                                                                      at android.app.Instrumentation.callActivityOnCreate(Instrumentation.java:1106)
                                                                      at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2278)
                                                                      at android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:2387) 
                                                                      at android.app.ActivityThread.access$800(ActivityThread.java:151) 
                                                                      at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1303) 
                                                                      at android.os.Handler.dispatchMessage(Handler.java:102) 
                                                                      at android.os.Looper.loop(Looper.java:135) 
                                                                      at android.app.ActivityThread.main(ActivityThread.java:5254) 
                                                                      at java.lang.reflect.Method.invoke(Native Method) 
                                                                      at java.lang.reflect.Method.invoke(Method.java:372) 
                                                                      at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:903) 
                                                                      at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:698) 
                                                             
                                                                   Caused by: java.lang.NoSuchMethodError: No static method setLayoutDirection(Landroid/graphics/drawable/Drawable;I)V in class Landroid/support/v4/graphics/drawable/DrawableCompat; or its super classes (declaration of 'android.support.v4.graphics.drawable.DrawableCompat' appears in /data/app/com.test.android-1/base.apk)
                                                                      at android.support.design.widget.CollapsingToolbarLayout.setStatusBarScrim(CollapsingToolbarLayout.java:663)
                                                                      at android.support.design.widget.CollapsingToolbarLayout.<init>(CollapsingToolbarLayout.java:197)
                                                                      at android.support.design.widget.CollapsingToolbarLayout.<init>(CollapsingToolbarLayout.java:132)
                                                                      at java.lang.reflect.Constructor.newInstance(Native Method) 
                                                                      at java.lang.reflect.Constructor.newInstance(Constructor.java:288) 
                                                                      at android.view.LayoutInflater.createView(LayoutInflater.java:607) 
                                                                      at android.view.LayoutInflater.createViewFromTag(LayoutInflater.java:743) 
                                                                      at android.view.LayoutInflater.rInflate(LayoutInflater.java:806) 
                                                                      at android.view.LayoutInflater.rInflate(LayoutInflater.java:809) 
                                                                      at android.view.LayoutInflater.rInflate(LayoutInflater.java:809) 
                                                                      at android.view.LayoutInflater.inflate(LayoutInflater.java:504) 
                                                                      at android.view.LayoutInflater.inflate(LayoutInflater.java:414) 
                                                                      at android.view.LayoutInflater.inflate(LayoutInflater.java:365) 
                                                                      at android.support.v7.app.AppCompatDelegateImplV7.setContentView(AppCompatDelegateImplV7.java:276) 
                                                                      at android.support.v7.app.AppCompatActivity.setContentView(AppCompatActivity.java:136) 
                                                                      at com.test.android.ui.activity.RefreshableActivity.onCreate(RefreshableActivity.java:31) 
                                                                      at android.app.Activity.performCreate(Activity.java:5990) 
                                                                      at android.app.Instrumentation.callActivityOnCreate(Instrumentation.java:1106) 
                                                                      at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2278) 
                                                                      at android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:2387) 
                                                                      at android.app.ActivityThread.access$800(ActivityThread.java:151) 
                                                                      at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1303) 
                                                                      at android.os.Handler.dispatchMessage(Handler.java:102) 
                                                                      at android.os.Looper.loop(Looper.java:135) 
                                                                      at android.app.ActivityThread.main(ActivityThread.java:5254) 
                                                                      at java.lang.reflect.Method.invoke(Native Method) 
                                                                      at java.lang.reflect.Method.invoke(Method.java:372)
```
**解决方案**
在项目的build.gradle文件中添加下面一行，同步一下即可
```
compile ('com.android.support:support-v4:23.4.0'){
    force = true;
}
```
[StackOverFlow解决方案](http://stackoverflow.com/questions/37423493/error-inflating-class-collapsingtoolbarlayout)


（18）**Android studio gradle编译异常**
```
java.lang.UnsupportedClassVersionError: com/android/build/gradle/AppPlugin : Unsupported major.minor version 52.0
```

很显然是class版本不支持。经查询，[Android](http://lib.csdn.net/base/android) Studio2.2必须使用JDK8及以上版本，而且是强制的。
所以呢，赶紧下了个JDK8最新版的。安装完毕，把JAVA_HOME指向了JDK8，实测JDK7和8是可以共存的。
那么，重启Android Studio后问题解决，Build Successful ！


（19）**电脑突然断电，Android studio 工程代码全部报错，找不到android sdk 的依赖包，clean、重启都没有用**
前几天公司搬家，正准备同步代码，突然断电、等把电脑搬到新办公楼，打开AS发现所有的项目代码报错，找不到android 依赖包，clean、重启都没有用，

（20）**recycleview嵌套列表项显示不全问题**
**解决方案：**
第一个RecyclerView的Adapter（即父RecyclerView）：
```
@Override  
    public MyHolder onCreateViewHolder(ViewGroup parent, int viewType) {  
               View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.shop_item,null); 解决条目显示不全  
               MyHolder holder = new MyHolder(view);  
                   return holder;  
     }  
```

第二个RecyclerView的Adapter（即子RecyclerView）：
```
@Override  
public MyHolder onCreateViewHolder(ViewGroup parent, int viewType) {  
    View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.check_item, parent,false);//解决宽度不能铺满  
   MyHolder holder = new MyHolder(view);  
      return holder;  
   }  
```

（21）**Android手机真机调试，日志不打印的解决方案：**
 ```
 1、在拨号界面输入：*#*#2846579#*#* 进入测试菜单界面。 
 2、Project Menu–后台设置–LOG设置
 3、LOG开关–LOG打开 LOG级别设置–VERBOSE 
 4、Dump&Log– 全部选中
 5、重启手机
 ```

（22）**java.lang.IndexOutOfBoundsException Inconsistency detected. Invalid item position 2(offset:2).state:4**
**解决方案：**Recyclerview在下拉刷新时，如果在数据没更新到之前将list  clear  之后，迅速滑动会造成crash，所以一般在下拉刷新之前，等数据刷新回来再把之前的数据进行清除。

（23）**使用友盟分享——微信、朋友圈分享出现java.lang.NoClassDefFoundError: org.apache.http.entity.mime.MultipartEntity
**
**解决方案:** 造成这样的原因是因为缺少httpmime_jar，添加是httpmime_jar包之后即可正常分享

（24）**Fragment中调用getActivity（）出现空指针异常**
**解决方案：**
 ```
   对于上面的问题，可以考虑下面这两种解决办法：
   1、不保存fragment的状态：在MyActivity中重写onSaveInstanceState方法，将super.onSaveInstanceState(outState);注释掉，让其不再保存Fragment的状态，达到fragment随MyActivity一起销毁的目的。
   2、重建时清除已经保存的fragment的状态：在恢复Fragment之前把Bundle里面的fragment状态数据给清除。方法如下：
        if(savedInstanceState!= null)
        {
            String FRAGMENTS_TAG = "android:support:fragments";
            savedInstanceState.remove(FRAGMENTS_TAG);
        }
 ```
（25）**RecyclerView嵌套使用切换页面出现自动滚动问题**
**原因：**
造成这样的原因是由于子RecyclerView抢占焦点导致的，如果你去查看RecyclerView的源码会发现，它会在构造方法中调用setFocusableInTouchMode(true)，所以，设为false可以解决这个问题。
**解决方案**
在子RecyclerView中调用如下方法
 ```
            //设置焦点不需要
            secondRvList.setFocusableInTouchMode(false);
            secondRvList.requestFocus();
 ```
（26）**Android 7.0设备拍照闪退问题**
**原因：**
Android 7.0 做了一些系统权限更改，为了提高私有文件的安全性，面向 Android 7.0 或更高版本的应用私有目录被限制访问，此设置可防止私有文件的元数据泄漏，如它们的大小或存在性。而此权限更改有多重副作用，其中之一就是当传递软件包网域外的 file:// URI 可能给接收器留下无法访问的路径。因此，尝试传递 file:// URI 会触发 FileUriExposedException。分享私有文件内容的推荐方法是使用 FileProvider。在应用间共享文件对于面向 Android 7.0 的应用，Android 框架执行的 StrictMode API 政策禁止在您的应用外部公开 file:// URI。如果一项包含文件 URI 的 intent 离开您的应用，则应用出现故障，并出现 FileUriExposedException 异常。要在应用间共享文件，应发送一项 content:// URI，并授予 URI 临时访问权限。进行此授权的最简单方式是使用 FileProvider 类。[点击查看Android官方说明](https://developer.android.google.cn/about/versions/nougat/android-7.0-changes.html)
**解决方案**
**1.在清单文件添加如下代码**
 ```
     <provider
            android:name="android.support.v4.content.FileProvider"
            android:authorities="你的应用包名.fileProvider"
            android:exported="false"
            android:grantUriPermissions="true">

            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/provider_paths"/>

        </provider>
 ```

 ```
android:authorities="com.alex.demo.FileProvider" 自定义的权限  
 ```

 ```
android:exported="false" 是否设置为独立进程  
 ```
 ```
android:grantUriPermissions="true" 是否拥有共享文件的临时权限  
 ```
 ```
android:resource="@xml/external_storage_root" 共享文件的文件根目录，名字可以自定义  
 ```
**2.在xml文件夹目录下新建provider_paths文件，名字自定义，添加如下代码**
 ```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <paths>
        <external-path
            name="camera_photos"
            path="" />
    </paths>
</resources>
 ```
**3.调用系统相机处代码处理**
 ```
   //调用系统相机拍照
Intent cameraIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
 if (cameraIntent.resolveActivity(getActivity().getPackageManager()) != null) {
  cameraIntent.putExtra(MediaStore.EXTRA_OUTPUT, parUri(tempFile));
  startActivityForResult(cameraIntent, REQUEST_CAMERA);
 }

   /**
     * 生成uri
     *
     * @param cameraFile
     * @return
     */
    private Uri parUri(File cameraFile) {
        Uri imageUri;
        String authority = getContext().getPackageName()+ ".provider";
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            //通过FileProvider创建一个content类型的Uri
            imageUri = FileProvider.getUriForFile(getContext(), authority, cameraFile);
        } else {
            imageUri = Uri.fromFile(cameraFile);
        }
        return imageUri;
    }

 ```

**（27）使用Glide加载列表项，刷新之后图片大小出现缩放问题**

**原因**：导致这样的问题是因为ImageView的默认资源大小和下载资源大小不一样。

**解决方案：**

（1）加载与Imageview 设置的宽高一致的图片，有的图片地址后面可以拼接对应的分辨率大小，然后根据传的分辨率大小来下载图片；

（2） 代码里面再设置一下ImageView的大小,然后再加载图片

 ```
/**
  *
  *此处的MyBitmapImageViewTarget 为自定义的BitmapImageViewTarget，在里面获取imagview的宽高
  **/
Glide.with(context)
.load(url)
.asBitmap()
.centerCrop()
.placeholder(loadingPic)
.error(errorPic)
.into(new MyBitmapImageViewTarget(imageView));
 ```
（3）禁止Glide的默认加载动画，也可以解决这个问题

 ```
Glide.with(context)..load(url).placeholder(R.drawable.icon_stub_dynamic).dontAnimate().into(imageView);
 ```

**（28）解决android 6.0(api 23) SDK，不再提供org.apache.http.(只保留几个类)**

 ```
 android {
    useLibrary 'org.apache.http.legacy'
}
 ```

**（29）'gradlew' 不是内部或外部命令，也不是可运行的程序或批处理文件。**
最近一个项目中需要使用到gradlew命令，Gradle相关环境变量也都配置好了，但就是在这个项目目录下运行gradlew命令不成功，相同的电脑另外一个项目目录下却可以运行成功，于是对比了一下两个项目的文件结构，发现缺少两个文件，如下所示，将这两个文件拷贝过来重新运行命令即可成功。

![image.png](https://upload-images.jianshu.io/upload_images/1956769-19057f3172f8d8ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**（30）Only fullscreen opaque activities can request orientation**

给activity启动页设置orientation后出现了错误主要是Android8.0上有异常，没有兼容。
 ```
1.找到你设置透明的Activity，然后在他的theme中将android:windowIsTranslucent改为false
<item name="android:windowIsTranslucent">false</item>
2.再加入<item name="android:windowDisablePreview">true</item>
3.去掉activity中的orientation属性
 ```

##如果觉得文章帮到你，喜欢我的文章可以关注个人微信公众号，将会定期推送优质技术文章，求关注~~~##

![欢迎关注“大话安卓”公众号](http://upload-images.jianshu.io/upload_images/1956769-2f49dcb0dc5195b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![欢迎加入“大话安卓”技术交流群，互相学习提升](http://upload-images.jianshu.io/upload_images/1956769-326c166b86ed8e94.JPG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


