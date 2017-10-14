---
title:  "StringBuffer和StringBuilder的区别"
date:   2016-04-06 14:30
categories: _posts
---

迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/239301016201636111215467/">《StringBuffer和StringBuilder的区别》</a>

就当作是面试题，个人在写代码的时候突然想起String这个字符类型的有很多种，以前也忘了在博客总结了，在这里简单总结一下，或许大家面试的时候有用处

1）在执行速度方面StringBuilder > StringBuffer

2）StringBuilder和StringBuffer都是可以改变的变量字符串对象，每当我们操作时都是在一个对象上操作的，不想String一样建立一些对象进行操作，所以速度快。

3）StringBuffer在Buffer缓冲区里面操作，所以是线程安全的，当我们在字符串缓冲区被多个线程使用的时候，JVM不能保证StringBuilder的操作是安全的，虽然他的速度快，但是可以保证StringBuffer是可以正确操作的。当然大多数情况下就是我们在单线程下进行操作的，所以大多数情况下建议使用StringBuiler 而不是用StringBuffer就是因为这个速度原因是。速度快，单线程安全，

建议：使用少量数据用String,单线程操作用字符串缓冲区使用大量数据StringBuilder,多线程操作字符串缓冲区大量数据使用StringBuffer。