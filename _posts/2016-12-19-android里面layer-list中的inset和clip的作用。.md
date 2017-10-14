---
title:  "android里面layer-list中的inset和clip的作用。"
date:   2016-12-19 14:30
categories: _posts
---

迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/239301016201611193924180/">《android里面layer-list中的inset和clip的作用。》</a>

Inset Drawable：用于通过指定的间距把图片插入到XML中，它在View需要比自身小的背景时常用。有些像padding的作用。例子：
　　第一步：drawable文件中建立inset_drawable.xml
　　<?xml version="1.0" encoding="utf-8"?>
　　<inset
　　xmlns:android="http：//schemas。android。com/apk/res/android"
　　android:drawable="@drawable/photo2"
　　android:insetTop="100dp"
　　android:insetRight="100dp"
　　android:insetBottom="200dp"
　　android:insetLeft="100dp" />

　　第二部，在xml中引用
　　<LinearLayout xmlns:android="http：//schemas。android。com/apk/res/android"
　　android:layout_width="match_parent"
　　android:layout_height="match_parent"
　　android:orientation="vertical"
　　android:background="@drawable/inset_drawable">

Clip Drawable：可以剪载图片显示，例如，可以通过它来做进度度。你可以选择是从水平或垂直方向剪载。其中的gravity设置从整个部件的哪里开始。例子：
　　第一步，在drawable文件中建立：clip_drawable.xml
　　<?xml version="1.0" encoding="utf-8"?>
　　<clip xmlns:android="http：//schemas。android。com/apk/res/android"
　　android:drawable="@drawable/test_img"
　　android:clipOrientation="horizontal"
　　android:gravity="left" />
　　第二步，在ImageView中引用：
　　<LinearLayout xmlns:android="http：//schemas。android。com/apk/res/android"
　　android:layout_width="match_parent"
　　android:layout_height="match_parent"
　　android:orientation="vertical">

　　<ImageView
　　android:id="@+id/clipimage"
　　android:layout_width="wrap_content"
　　android:layout_height="wrap_content"
　　android:src="@drawable/clip_drawable"/>
　　</LinearLayout>

在ImageView中设置的是android:background="@drawable/clip_drawable"，但是我使用background的时，会在程序中报空指针的错误。
　　最后，使用程序控制：
　　ImageView imageView=(ImageView)findViewById(R.id.clipimage);
　　ClipDrawable clipDrawable=(ClipDrawable)imageView.getDrawable();
　　clipDrawable.setLevel(5000);
　　level的值为0到10000 。当值为10000时图全部显示。