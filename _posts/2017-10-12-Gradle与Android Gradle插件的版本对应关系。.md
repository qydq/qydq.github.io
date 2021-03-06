---
title:  "Gradle与Android Gradle插件的版本对应关系。"
date:   2017-10-12 20:32
categories: _post
---

# Gradle可以作为Android studio中的插件使用。

当更新Andorid studio 的时候，你可能会在Android Studio中接收到一条让你更新Gradle插件到最新版本，点击更新可以在IDE内下载最新的Gradle 插件。

或者当你想提升编译速度你就需要手动去下载最新的gradle插件，应用到你的项目中，官网下载地址在最后给出。

下图的表格是Andorid Gradle插件的版本和Gradle版本的对应关系。为了最好的编译表现，你应该使用最新的Gradle插件版本和与之对应的最新的Andorid Gradle 插件的版本。

| Plugin version        | Required Gradle version    | 
| :--------:   | :-----:   | 
| 1.0.0 - 1.1.3       | 2.2.1 - 2.3      | 
| 1.2.0 - 1.3.1      | 2.2.1 - 2.9      | 
| 1.5.0      | 2.2.1 - 2.13      | 
| 2.0.0 - 2.1.2      | 2.10 - 2.13      | 
| 2.1.3 - 2.2.3      | 2.14.1+      |
| 2.3.0+      | 3.3+      |

*注释:左侧是Andorid gradle插件的版本，右侧是gradle的版本。*

# 那么在哪里指定Andorid gradle的插件版本呢？

（1）在最外层的build.gradle文件中
```groovy
buildscript {
  ...
  dependencies {
    classpath 'com.android.tools.build:gradle:2.3.2'
  }
}
```
（2）Andorid studio 中选择File > Project Structure > Project 

# Gradle插件下载的地址

在你项目的gradle文件下/wrapper/gradle-wrapper.properties 打开就可以看到如下内容：
或者我的电脑：C:\Users\sunshuntao\.gradle\wrapper\dists\gradle-2.4-all

```groovy
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-2.2.1-all.zip
```

distributionUrl 为[gradlew插件下载地址](https://services.gradle.org/distributions) 

https://services.gradle.org/distributions

gradle plugin 和 android gradle plugin version对应关系在网上搜一大片，可是

# 那么在哪里能找到最新的版本对应关系呢？

[Gradle与Android Gradle插件的版本对应关系-官网地址](https://developer.android.com/studio/releases/gradle-plugin.html#updating-gradle)

https://developer.android.com/studio/releases/gradle-plugin.html#updating-gradle


备注： 需要自备梯子（qiang） 。


```groovy
The run dex in process, the Gradle daemon needs a larger heap.
It currently has 2048 MB.
For faster builds, increase the maximum heap size for the Gradle daemon to at least 4608 MB (based on the dexOptions.javaMaxHeapSize = 4g).

To do this set org.gradle.jvmargs=-Xmx4608M in the project gradle.properties.
```

---晴雨【qy】-- 
