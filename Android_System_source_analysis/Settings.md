# Settings整体代码结构以及语法格式特点
内容：

- Settings相关代码位置
- 语法格式特点

## １.Settings整体代码结构位置
- 针对与Settings的相关定制工作，首先明确Settings代码的存放位置，及目录结构
- 根目录／packages/app 下，有多款应用，其中Settings即我们要操作的设置应用
- 整体目录结构：Android.mk  AndroidManifest.xml  CleanSpec.mk  MODULE_LICENSE_APACHE2  NOTICE  proguard.flags  res  src  tests
- 其中Android.mk类似于加载类，在运行前会通过加载它来加载众多的jar等
- AndroidManifest.xml就是正常的清单配置文件，比如Settings的主入口等，都可以在此处找到
- CleanSpec.mk　用于clean编译时重新编译生成代码
- 至于res是settings的布局文件，src为代码文件，所有的settings内容都可以在这部分文件中找到

## ２.语法格式特点
- Settings中的语法实现和主流Android开发有些许差别，它的整体实现依旧依赖与Activity和Fragment,但它的主要布局实现依托于preference方式，代码基于此实现
- 关于Preference相关语法，可参考：http://blog.csdn.net/wujiangming/article/details/6218068
　　　　　　　　　　　　　　　　http://blog.csdn.net/xfks55/article/details/6528269
　　　　　　　　　　　　　　　　http://www.cnblogs.com/feike/archive/2013/01/06/2847026.html
- 主界面的布局文件放在res/xml中，名dashboard_categories.xml，通过接点击某一项调起特定的Fragment
     类似于：
``` 
<dashboard-tile
                android:id="@+id/wifi_settings"
                android:title="@string/wifi_settings_title"
                android:fragment="com.android.settings.wifi.WifiSettings"
                android:icon="@drawable/ic_settings_wireless"
                />
``` 
- 针对与不同功能布局：
　类似：　
``` 
<PreferenceScreen
                android:key="wallpaper"
                android:title="@string/wallpaper_settings_title"
                settings:keywords="@string/keywords_display_wallpaper"
                android:fragment="com.android.settings.WallpaperTypeSettings" />
``` 

　可以通过key值找到到该应用，通过findPreference(key)　的方式，上述例子可以通过点击直接启动相应的fragment，Activity可以做同样处理
　对于其他的控件，自己可以学习一下，不详细列出了
- 接下的功能实现，方式与一般无异，有个小细节，对于Settings中的代码，想使用某些内部类，无需反射调用可以直接使用。
