---
title:  "Unable to delete file Build"
date:   2016-11-03 14:30
categories: _posts
---

迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/239301016201610354915678/">《Unable to delete file: E:\caibao\dangkelanqiu\Dunk\app\build\intermediates\exploded-aar\com.android.support\recyclerview-v7\23.1.1\jars\classes.jar 》</a>

<p><wbr>android studio 提一个bug（sunshuntao）</p><pre>解决这个问题，什么关闭android都是乱说，我clean一下，有出错了，非要我关闭电脑才行吗，在这里说no</pre>
<p>&nbsp;</p>
<p>&nbsp;</p>
<div><img title="Unable to delete file: E:caibaodangkelanqiuDunkappbuildintermediatesexploded-aarcom.android.supportrecyclerview-v723.1.1jarsclasses.jar - Nakama - Bgwan" style="margin: 0px 10px 0px 0px;" alt="Unable to delete file: E:caibaodangkelanqiuDunkappbuildintermediatesexploded-aarcom.android.supportrecyclerview-v723.1.1jarsclasses.jar - Nakama - Bgwan" src="http://img2.ph.126.net/85TeBQ6ncop4tZzXxHD-1Q==/6632112301745308811.png"></div>
<p><strong>&nbsp;古今百度了所有，都没有遇到解决的办法，看来百度真的不行了，觉得不能用百度。。解决办法在最后，不能解决你打我。</strong></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<div><img title="Unable to delete file: E:caibaodangkelanqiuDunkappbuildintermediatesexploded-aarcom.android.supportrecyclerview-v723.1.1jarsclasses.jar - Nakama - Bgwan" style="margin: 0px 10px 0px 0px;" alt="Unable to delete file: E:caibaodangkelanqiuDunkappbuildintermediatesexploded-aarcom.android.supportrecyclerview-v723.1.1jarsclasses.jar - Nakama - Bgwan" src="http://img2.ph.126.net/Jb099DB2rUtjt-nAdhJWQQ==/6632109003210425360.jpg"></div>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>1，没有权限，这样就是不是有了权限了</p><pre><strong>task clean(type: org.gradle.api.tasks.Delete) {<br>    delete rootProject.buildDir<br>}</strong></pre><pre>&nbsp;</pre>