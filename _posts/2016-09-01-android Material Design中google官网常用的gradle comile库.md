---
title:  "android Material Design中google官网常用的gradle comile库"
date:   2016-09-01 14:30
categories: _posts
---

迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/239301016201681114450348/">《android Material Design中google官网常用的gradle comile库》</a>

android Material Design中google官网常用的gradle comile库 ，在app /moudle 的build.gradle加入以下依赖。

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12' //java junit测试类
    compile(name: 'AndroidUtils', ext: 'aar') //libs下面aar AndroidUtils文件的依赖库
    compile(name: 'awaarssnst-debug', ext: 'aar') //libs下面awaarssnst-debug依赖库
//    compile project(":AndroidUtils") //项目lib的AndroidUtils的依赖库
compile 'cn.sunst.android:aw_bintray_aar:1.0'//aw的私人依赖库
compile 'com.github.qydq:aw-jitpack:1.0.1'//aw的私人依赖库
compile(name: 'aw_bintray_qydq-debug', ext: 'aar')//aw的私人依赖库
注意：以上3可以组合使用，效果更好，因为决定权在你的手中。
    compile 'com.android.support:appcompat-v7:23.2.1' //appcompat-v7:23.2.1依赖库Meterial Desigin
    compile 'com.android.support:recyclerview-v7:23.2.1'// recycleview-v7:23.2.1依赖库MD
    compile 'com.android.support:cardview-v7:23.1.1' //cardview-v7:23.1.1依赖库MD
compile 'com.android.support.constraint:constraint-layout:1.0.0-alpha7'//约束布局的依赖库MD
}