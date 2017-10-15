---
title:  "android 内存操作打印数据说明(补充mount方法)"
date:   2017-03-21 18:50:48
categories: _posts
---


迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/239301016201722145923554/">
android 内存操作打印数据说明(补充mount方法)
</a>



Environment.getDataDirectory().getPath() :                                      获得根目录/data 内部存储路径
Environment.getDownloadCacheDirectory().getPath()  :               获得缓存目录/cache
Environment.getExternalStorageDirectory().getPath():                  获得SD卡目录/mnt/sdcard（获取的是手机外置sd卡的路径）
        Environment.getRootDirectory().getPath() :                                     获得系统目录/system

        通过Context获取的
Context.getDatabasePath()                                                      返回通过Context.openOrCreateDatabase 创建的数据库文件
Context.getCacheDir().getPath() :                                            用于获取APP的cache目录 /data/data/<application package>/cache目录
Context.getExternalCacheDir().getPath()  :                           用于获取APP的在SD卡中的cache目录/mnt/sdcard/Android/data/<application package>/cache
Context.getFilesDir().getPath()  :                                             用于获取APP的files目录 /data/data/<application package>/files
Context.getObbDir().getPath():                                                用于获取APPSDK中的obb目录 /mnt/sdcard/Android/obb/<application package>
        Context.getPackageName() :                                                  用于获取APP的所在包目录
Context.getPackageCodePath()  :                                          来获得当前应用程序对应的 apk 文件的路径
Context.getPackageResourcePath() :                                   获取该程序的安装包路径




/*
* android系统可通过Environment.getExternalStorageDirectory()获取存储卡的路径，
* 但是现在有很多手机内置有一个存储空间，同时还支持外置sd卡插入，
* 这样通过Environment.getExternalStorageDirectory()方法获取到的就是内置存储卡的位置，
* 需要获取外置存储卡的路径就比较麻烦，这里借鉴网上的代码，稍作修改，
* 在已有的手机上做了测试，效果还可以，当然也许还有其他的一些奇葩机型没有覆盖到。
*
* 得到mount外置sd卡的路径（复杂参数）
* */
public String getMountComplexSdcardPath() {
    String sdcard_path = null;
    String sd_default = Environment.getExternalStorageDirectory()
            .getAbsolutePath();
    if (sd_default.endsWith("/")) {
        sd_default = sd_default.substring(0, sd_default.length() - 1);
    }
    // 得到路径
    try {
        Runtime runtime = Runtime.getRuntime();
        Process proc = runtime.exec("mount");
        InputStream is = proc.getInputStream();
        InputStreamReader isr = new InputStreamReader(is);
        String line;
        BufferedReader br = new BufferedReader(isr);
        while ((line = br.readLine()) != null) {
            if (line.contains("secure"))
                continue;
            if (line.contains("asec"))
                continue;
            if (line.contains("fat") && line.contains("/mnt/")) {
                String columns[] = line.split(" ");
                if (columns != null && columns.length > 1) {
                    if (sd_default.trim().equals(columns[1].trim())) {
                        continue;
                    }
                    sdcard_path = columns[1];
                }
            } else if (line.contains("fuse") && line.contains("/mnt/")) {
                String columns[] = line.split(" ");
                if (columns != null && columns.length > 1) {
                    if (sd_default.trim().equals(columns[1].trim())) {
                        continue;
                    }
                    sdcard_path = columns[1];
                }
            }
        }
    } catch (Exception e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }
    return sdcard_path;
}
