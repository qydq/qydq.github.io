<link href="markdown.css" rel="stylesheet"></link>
# 通用adb命令[adb_command.md]
多系统通用adb操作命令，创建时间：2019-06-25 21:54，作者：sunst。

**ps:adb命令中如**
>com.sunst.ff/com.sunst.ff.TestActivity

1.com.sunst.ff是程序的包名。
2.TestActivity是程序Activity类名。
## 写在前面：
小团子是我这一辈子最爱的人，希望有一天能再次跟她重逢winter is comming

本备忘录📕：记录android中adb多系统操作相关命令anLff[anLff]

本备忘录📕：同步github/qydq.github.io/:=[adb_command.md](https://qydq.github.io/adb_command.md)
## 日志语言正反斜杠解释
**日志：**
>Log.d("sunst888" + TAG, "into onConnectionSuspended : ");

[**oobe中设置系统语言可以参考：**](https://zhihu.com/people/qydq)
```
Locale local = new Locale("zh", "CN");
new Locale("zh","TW");
new Locale("en");
new Locale("en","US");
new Locale("en","UK");
new Locale("ja", "JP");
new Locale("ar","SA");

getResources().getConfiguration().locale.getCountry().equals("TW")
```
**正反斜杠：**
* 正斜杠 : ///////////
* 反斜杠 : \\\\\\\\\\\

在Windows系统中，正斜杠/表示除法，用来进行整除运算；反斜杠\用来表示目录。

在Unix系统中，正斜杠/表示目录；反斜杠\表示跳脱字符将特殊字符变成一般字符（如enter,$,空格等）

**注：**
>不管是pull还是push对device来说，目录路径使用反斜杠\\\\\\\\
## 一：adb常用命令
**ps:**
>常用命令和后面总结的命令有重复是正常的
### 1.早期常用命令
**mac电脑操作技巧**
>commond+shift+G,跳出前往文件夹的窗口

**window**
>msconfig修改系统引导
>命令窗口快捷打开windows+R

**说明：以下操作是一样的**
>adb shell rm -rf /data/app/com.huawei.recsys/base.apk
>adb shell
>rm -rf /data/app/com.huawei.recsys/base.apk

#### (1).查看是否连接手机
>adb devices
#### (2).将设备改为可读可写
>adb remount

显示remount succeeded就代表命令执行成功；
#### (3).进入指定的device的shell
>adb shell

或者
>adb -s ********* shell
#### (4).查看当前目录命令
>pwd
#### (5).adb查看所有安装的包
>pm list packages
#### (6).根据某个关键字查找包
>pm list packages | grep tencent
#### (7).查看包安装位置
>pm list packages -f
>pm list packages -f | grep tencent
>adb shell pm list packages -f | grep tencent
#### (8).adb push
* 把E盘folder文件夹 拷贝到设备sdcard目录xfolder下（xfolder为新建
>adb push E:\sunst\folder /sdcard/xfolder

Tips：folder中的文件love.txt也放进去在xfolder目录

* 将folder下的文件夹拷贝到机器sunst文件夹中
>adb push E:\sunst\folder\. /sdcard/sunst

* 将folder整个文件拷贝到sunst文件夹中
>adb push E:\sunst\. /sdcard/sunst

* 将文件love.txt拷贝到sunst文件夹中并重新命名为newlovet.txt
>adb push E:\sunst\test.txt sdcard/sunst/copytest.txt
#### (9).adb pull
>adb pull sdcard/sunst/copytest.txt E:/sunst/
>adb pull sdcard/sunst/copytest.txt E:\sunst\xixi.txt 并重命名

>adb pull /data/app/com.tencent.tbs-1/base.apk ~ 直接把设备文件拷贝到根目录

>adb pull sdcard\sunst\copytest.txt（错误写法)

**备注：**
>**不管是pull还是push对device来说，目录路径使用反斜杠**
#### (10).adb命令查询apk是否在运行
>linux下：adb shell ps | grep [apk包名]
>windows下：adb shell ps | findstr [apk包名]

执行命令后如果有显示你搜索的apk包名，那说明正在运行，否则就是没有运行
#### (11).当前目录下查找文件
* 查找文件中带有should_search_name字段
>find . -name "*.java" | xargs grep -ir "should_srearch_name"

* 查找文件
>find . -name "sunst"
>find . -name "*un*"#使用通配符

**说明:**
>find命令用于查找文件，后面的“."代表当前目录，-name是find命令的参数，后面接要搜索的文件名。
#### (12).adb查看当前栈顶Activity
>adb shell dumpsys activity | grep "Run"
### 2.后期常用命令
adb reboot bootloader
fastboot devices
fastboot continue

一次烧写boot，system，recovery分区，创建包含boot.img，system.img，recovery.img文件的zip包。

>执行：fastboot update {*.zip}

**注：**华为手机解锁命令: fastboot oem unlock 解锁码
#### (1)、快速查看SettingsProvider数据库表信息：
>adb shell settings list [system][secure][global]

#### (2)、依据Name查看SettingsProvider数据库表格
>adb shell settings get [system][secure][global] [Name]

#### (3)、依据Name向对应的数据库中插入value
>adb shell settings put [system][secure][global] [Name] [Value]

#### (4)、查看启动页面intent部分详情以及确认请求部分详情
>adb logcat -v time -s "ActivityManager"

### 3.adb命令注意事项
(1).push某个文件到目标板时，可通过如下命令，将目标目录临时变更为可读写模式：
* 解决方法（1）：
>adb shell
>mount -o remount -rw /system

* 解决方法（2）：
>adb root
>adb remount

## 二：adb高级命令
### 1.dump命令相关
#### (1).列出所有
>adb shell dumpsys

#### (2).获得手机里面某个apk的应用信息、版本信息
>adb shell dumpsys package com.sunst.ff.packagename
>adb shell dumpsys package > ./package.txt

#### (3).获取包名对应的PMS信息
>adb shell dumpsys package {包名}

#### (4).显示当前展示页面的信息，可以用于确认启动activity的包名等信息
>adb shell dumpsys window visible

#### (5).显示activity详情
>adb shell dumpsys activity com.android.settings/com.android.settings.Settings

#### (6).显示系统组件详情
* 显示服务详情
>adb shell dumpsys activity service com.android.settings/.SettingsDumpService

* 显示系统服务详情
>adb shell dumpsys notification

* 显示Provider服务详情：
>adb shell dumpsys activity provider com.android.providers.settings/com.android.providers.settings.SettingsProvider

#### (7).抓取手指点击事件时间
>adb logcat -v time -s InputDispatcher

**输出案例：**
```
07-29 11:04:56.258 I/ViewRootImpl( 8295): finishMotionEvent: handled = true stage=10:
View Post IME stage,inputElapseTime=2 eventTime = 68608867
downTime = 68608867 title= com.android.launcher3/com.android.launcher3.Launcher
```
#### (8).输出AMS intent请求信息：
>adb logcat -v time -s "ActivityManager"

**输出案例：**
```
07-29 11:29:48.520 I/ActivityManager( 6692): START u0 {act=android.intent.action.MAIN
cat=[android.intent.category.LAUNCHER] flg=0x10200000
mCallingUid=10012 cmp=com.android.settings/.Settings bnds=[1056,1804][1392,2222] (has extras)}
from uid 10012 on display 0
```
#### (9).输出页面启动时间
>adb logcat -v time -s "ActivityManager" |grep Display

**输出案例：**
```
07-29 11:18:16.557 I/ActivityManager( 1579): Displayed com.android.settings/.SubSettings: +299ms
```
#### (10).开机向导不正常导致HOME键和返回键不可用修正方案：
>adb shell settings put global device_provisioned 1
>adb shell settings put secure user_setup_complete 1

#### (11).获得Intent请求来源以及activityResult目标详细log信息
>[adb shell dumpsys activity a] [adb shell dumpsys activity]
>adb shell dumpsys activity -d list：

**输出案例：**
```
ENABLE_THERMAL = true
0 . DEBUG_ALL = false
1 . DEBUG_BACKUP = false
2 . DEBUG_BROADCAST = false
3 . DEBUG_BROADCAST_LIGHT = false
4 . DEBUG_BROADCAST_BACKGROUND = false
5 . DEBUG_CLEANUP = false
6 . DEBUG_CONFIGURATION = false
7 . DEBUG_FOCUS = false
8 . DEBUG_IMMERSIVE = false
9 . DEBUG_LOCKSCREEN = false
10. DEBUG_LRU = false
11. DEBUG_MU = false
13. DEBUG_OOM_ADJ = false
14. DEBUG_PAUSE = false
15. DEBUG_POWER = false
16. DEBUG_POWER_QUICK = false
17. DEBUG_PROCESSES = false
18. DEBUG_PROCESS_OBSERVERS = false
19. DEBUG_PROVIDER = false
20. DEBUG_RESULTS = false
21. DEBUG_SERVICE = false
22. DEBUG_SERVICE_EXECUTING = false
23. DEBUG_STACK = true
24. DEBUG_SWITCH = true
25. DEBUG_TASKS = true
26. DEBUG_THUMBNAILS = false
27. DEBUG_TRANSITION = false
28. DEBUG_URI_PERMISSION = false
29. DEBUG_USER_LEAVING = false
30. DEBUG_VISIBILITY = false
31. DEBUG_PSS = false
32. DEBUG_RECENTS = false
33. DEBUG_THERMAL = false
34. DEBUG_ADD_REMOVE = false
35. DEBUG_APP = false
36. DEBUG_CONTAINERS = false
37. DEBUG_IDLE = false
38. DEBUG_RELEASE = false
39. DEBUG_SAVED_STATE = false
40. DEBUG_SCREENSHOTS = false
41. DEBUG_STATES = false
42. DEBUG_VISIBLE_BEHIND = false
```
**ps:**

可以选择将如下三项打开：

23. DEBUG_STACK = true
24. DEBUG_SWITCH = true
25. DEBUG_TASKS = true

#### (12).使用如下命令抓取log信息：
>adb logcat -v time -s "ActivityManager"

**log备注：**
```
01-02 05:47:11.624 12182 13492 I ActivityManager: START u0 {mCallingUid=1000 cmp=com.android.settings/.ChooseLockPattern (has extras)} from uid 1000 on display 0
01-02 05:47:11.624 12182 13492 I ActivityManager: sourceRecord = ActivityRecord{82c19d8 u0 com.android.settings/.ChooseLockGeneric$InternalActivity, isShadow:false t3}
01-02 05:47:11.624 12182 13492 D ActivityManager: startActivityLocked skipCheckAccessControl = true
01-02 05:47:11.634 12182 13492 I ActivityManager: startActivityUnchecked mReusedActivity=null r=ActivityRecord{a5b9d3b u0 com.android.settings/.ChooseLockPattern, isShadow:false t-1}
01-02 05:47:11.636 12182 13492 V ActivityManager: Starting new activity ActivityRecord{a5b9d3b u0 com.android.settings/.ChooseLockPattern, isShadow:false t3} in existing task TaskRecord{9629e54 #3 A=com.android.settings, isShadow:false U=0 StackId=1 sz=4} from source ActivityRecord{82c19d8 u0 com.android.settings/.ChooseLockGeneric$InternalActivity, isShadow:false t3}
```

**ps：**

>SourceRecord=开头的字串

#### (13).事件被消费：
>adb logcat -v threadtime -s ViewRootImpl

**可以看到如下log：**
```
04-25 01:57:21.441 11173 11173 I ViewRootImpl: finishKeyEvent: handled = true keycode = 4 name=KEYCODE_BACK stage=10: View Post IME stage,inputElapseTime=4 eventTime = 4616876 downTime = 4616876 title= com.android.settings/com.android.settings.Settings
04-25 01:57:21.840 2920 2920 I ViewRootImpl: updateImmersive mode: gain focus
```

#### (14).随机产生事件给某个应用程序, 随机产生500个事件给程序。你会发现你的程序不断的被点击，旋转！
>adb shell monkey -p com.sunst.ff.pakagename -v 500

### 2.pm命令相关

>adb shell
>pm grant com.provision.wearprovision.activity android.permission.CHANGE_CONFIGURATION

>adb shell pm grant com.provision.wearprovision.activity android.permission.CHANGE_CONFIGURATION

pm工具为包管理（package manager）的简称，可以使用pm工具来执行应用的安装和查询应用包的信息、系统权限、控制应用

>pm工具是Android开发与测试过程中必不可少的工具

#### (1).shell命令格式：
>pm [command]

#### (2).包名信息查询
>pm list packages [options] [FILTER]

打印所有的已经安装的应用的包名，如果设置了文件过滤则值显示包含过滤文字的内容

| 参数 |描述 |
|------------|---------------------------------|
|-f |`'显示每个包的文件位置'` |
|-d |`"使用过滤器，只显示禁用的应用的包名"` |
|-e |`使用过滤器，只显示可用的应用的包名`|
|-s |`使用过滤器，只显示系统应用的包名` |
|-3 |`使用过滤器，只显示第三方应用的包名`|
|-i |`查看应用的安装者`|

#### (3).权限基础
```
<permission android:description="string resource"
android:icon="drable resource"
android:label="string resource"
android:name="string"
android:permissionGroup="string"
android:protectionLevel=["normal"|"dangerous"|"signature"|"signatureOrSystem"]/>
```
protectionLevel

**说明**

normal 表示权限是低风险的，不会对系统，用户或其他应用程序造成危害
dangerous 表示权限是高风险的，系统将可能要球用户输入相关信息，才会授予此权限
signature 表示只有当应用程序所用数字签名与声明引用权限的应用程序所用签名相同时，才能将权限授予给它
signatureOrSystem 需要签名或者系统级应用（放置在/system/app目录下）才能赋予权限
system 系统级应用（放置在/system/app目录下）才能赋予权限
自定义权限 应用自行定义的权限

#### (4).权限查询

* 打印所有已知的权限组
>pm list permission-groups

* 打印权限
>pm list permissions [options] [GROUP]

**Tips:**

参数可以组合使用例如：pm list permissions –g -d
| 参数 |描述 |
|------------|---------------------------------|
|-g |`按组进行列出权限` |
|-f |`打印所有信息` |
|-s |`简短的摘要`|
|-d |`只有危险的权限列表` |
|-u |`只有权限的用户将看到列表用户自定义权限`|

#### (5).授权与取消

* 授权
>pm grant <packageName> android.permission.READ_CONTACTS

* 取消
>pm revoke <packageName> android.permission.READ_CONTACTS

**Tips:**

所谓的授权是指你的apk里面已有的权限进行授权，相当于启用的概念

#### (6).测试包与apk路径查询

* 列出所有的instrumentation测试包
>pm list instrumentation

* 打印指定包名的apk路径
>pm path PACKAGE_NAME

#### (7).系统功能与支持库查询

* 打印系统的所有功能列出所有硬件相关信息
>pm list feature
*打印当前设备所支持的所有库
>pm list libraries

#### **(8).打印包的系统状态信息

>pm dump PACKAGE

#### (9).安装与卸载

* 安装
>pm install [-lrtsfd] [-i PACKAGE] [PATH]

通过指定路径安装apk到手机中(与adb install不同的是adb install安装的.apk是在你的电脑上，而pm install安装的apk是存储在你的手机中)

首先将test.apk文件push到手机目录中比如/data/local/tmp

>adb shell pm install /data/local/tmp/test.apk #安装
>adb shell pm install –r /data/local/tmp/test.apk #重新安装

| 参数 |描述 |
|------------|---------------------------------|
|-l |`锁定应用程序` |
|-r |`重新安装应用，且保留应用数据` |
|-t |`允许测试apk被安装`|
|-i |`指定安装包的包名` |
|-s |`安装到sd卡`|
|-f |`安装到系统内置存储中（默认安装位置）`|
|-d |`允许降级安装（同一应用低级换高级）`|
|-g |`授予应用程序清单中列出的所有权限（只有6.0系统可用）`|

* 卸载

>pm uninstall [options] <PACKAGE>

参数-k 卸载应用且保留数据与缓存（如果不加-k则全部删除）

#### (10).其它相关命令

* 清除应用数据
>pm clear <PACKAGE_NAME>

* 禁用和启用应用
>pm enable <PACKAGE_OR_COMPONENT>可用
>pm disenable <PACKAGE_OR_COMPONENT>不可用（直接就找不到应用了）
>pm disenable-user [options] <PACKAGE_OR_COMPONENT>不可用（会显示已停用)

* 隐藏与恢复应用
pm hide PACKAGE_OR_COMPONENT 隐藏package或component
pm unhide PACKAGE_OR_CONPONENT恢复可见package或component

**Tips:**

被隐藏应用在应用管理中变得不可见，桌面图标也会消失

### 3.am命令相关
#### (1).用adb启动apk
* 启动1
>adb shell am start -n com.sunst.ff/com.sunst.ff.MainActivity

* 启动2
>adb shell monkey -p com.sunst.ff -c android.intent.category.LAUNCHER 1

* 启动3
>adb shell am start -a android.intent.action.MAIN -c android.intent.category.LAUNCHER -n com.sunst.ff.packagename/com.sunst.ff.packagename.TestActivity
>adb shell am start -n com.sunst.ff/com.sunst.ff.TestActivity

* 启动4
>adb shell am start -n com.sunst.ff/.TestActivity

#### (2)adb关闭
>adb shell am force-stop 包名

### 4.ota命令相关
ota是空中下载技术，就是完成手机升级的术语，具体可参考[我知乎](https://zhihu.com/people/qydq)，

**在操作中，可以通过部分重启来节省时间。在cmd中执行如下命令：**
>abd shell//进入adb shell 模式
>am restart //重启系统（非完全重启）

#### (1).手机正常启动后，命令行模式下输入
>adb reboot bootloader

该命令会自动进入```fastboot模式```
#### (2).接着：
>fastboot devices

查看是否有设备 或者adb devices
#### (3).erase 擦除命令
>fastboot erase system
>fastboot erase cache
>fastboot erase config
>fastboot erase data
>fastboot erase logs
>fastboot erase factory
#### (4).格式化
>fastboot format data # 格式化 data 分区
#### (5).加载一些镜像
>fastboot flash boot boot.img # 刷入 boot 分区

>fastboot flash system system.img # 刷入 system 分区

>fastboot flash recovery recovery.img # 刷入 recovery 分区

>fastboot flashall #烧写所有分区，注意：此命令会在当前目录中查找所有img文件，将这些img文件烧写到所有对应的分区中，并重新启动手机。
#### (6). 设备锁
>fastboot flashing lock # 设备上锁，刷机完毕
>fastboot flashing unlock #6.0以上设备 设备必须解锁，开始刷机（这个不同的手机厂商不同）
#### (7).重启
>fastboot reboot
#### (8).自动重启设备
>fastboot continue
#### (9).重启到bootloader，刷机用
>fastboot reboot-bootloader
#### (10).其它命令
也可以采用adb shell模式下的```dd命令```来刷recovery.img
>adb shell
>su

* 高通平台：
>dd if=/data/local/tmp/recovery.img of=/dev/block/platform/msm_sdcc.1/by-name/recovery

* MTK平台：
>dd if=/data/local/tmp/recovery.img of=/dev/recovery

* 英伟达nvidia平台：
>dd if=/data/local/tmp/recovery.img of=/dev/block/platform/sdhci-tegra.3/by-name/SOS

>reboot recovery

**Tips：**
>adb reboot recovery 也可以让手机开机进入```recovery模式```

===================================================================================================