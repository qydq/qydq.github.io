---
title:  "如何转载大图像BitmapFactory"
date:   2015-04-16 16:57:54
categories: _posts
---

迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/239301016201531663451332/">《如何转载大图像BitmapFactory》</a>

提示方法

android.graphics.BitmapFactory类对象有静态方法

Bitmap bitmap = BitmapFactory.decodeFile("/sdcard/adb.jpg");

装载的图像大小希望暂时不要超过1MB不然 毕竟是手机

android.graphic.BitmapFactory.Options 类有一个缩放的功能

Option options = new Options();

optims.inSampleSize = 4 ;代表比例为4分分之一

Bitmap bit = BitmapFacotry.decoldeFile("/ascard/bgwan.jpg",options);

最后ImageView.setImageBitmap()直接使用

