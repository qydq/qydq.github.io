---
title:  "android studio 发布开源项目到jcenter$bintray"
date:   2017-03-28 18:50:48
categories: _posts
---


迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/239301016201722824311387/">
android studio 发布开源项目到jcenter$bintray
</a>

<div>知乎为本人知乎：原文地址（<a target="_blank" rel="nofollow" href="https://zhuanlan.zhihu.com/p/26025724">Bgwan</a>）：https://zhuanlan.zhihu.com/p/26025724</div><div><br></div><div><p>爱一个人，希望她过更好，打从心里暖暖的，你比自己更重要 ，张小月啊，张小月。</p><p>前言：</p><br><p>因为一些事，已经接近三个月没有更新过技术分享总结了。</p><br><p>去年大概六七月份到今天的时候做了两个开源的项目，an-aw-base和an-aw-zxing，前面一个是基于自己的an-maven-base优化而来，主要是帮助开发者快速开发android应用程序，后面一个是基于an-maven-zxing优化而来，主要是集成了google的开源项目zxing二维码的扫描功能。更多信息请参考我的github地址：<a target="_blank" rel="nofollow" href="https://link.zhihu.com/?target=https%3A//github.com/qydq">qydq (晴雨荡气（莳萝花 ）)<i></i></a>。说这么多也是想给自己宣传一下，这两个框架都支持jetpack和jcenter直接一行代码利用Gradle去依赖 。</p><p>ok,it's beginning  ,</p><p>上面的两个开源项目，一开始发布的方法使用了很笨的方法，也就是我专栏《Android开发》介绍的如何打包aar到 ，用到的命令是 ：gradlew clean build -generateRelease ，这句命令会在你的库文件的/build下面生成app-realease1.x.x.zip文件，然后我再利用生成的这个文件直接在bintray上面把项目上传上去。这样做很是费劲 ，当初也知道<a target="_blank" rel="nofollow" href="https://link.zhihu.com/?target=http%3A//my.csdn.net/yanzhenjie1003">严振杰<i></i></a>写了“AndroidStuio快速发布开源项目到Jcenter/Bintray”，当时想着以后再说，没想到差不多过了七八个月的时间，</p><p>利用笨的方法，gradlew clean build -generateRelease 编译的时间差不多都要10多分钟，那种方法也可以支持gradle依赖，但是今天决定摒弃这种笨（耗时）的方法，直接在android-studio中集成 。</p><p>这里我的示例项目是an-aw-base ，就当作更新另外一个版本，而且an-aw-base这次也做了升级，对android7.0以上系统当初没有考虑到兼容性会出现SecurityException或者FileUriExposedException。所以需要升级高的版本。</p><p>这里还是参考严正杰前辈的分享，非常止血，个人做了部分修改，原文地址：<a target="_blank" rel="nofollow" href="https://link.zhihu.com/?target=http%3A//blog.csdn.net/yanzhenjie1003/article/details/51672530">AndroidStuio快速发布开源项目到Jcenter/Bintray - 严振杰 - 博客频道 - CSDN.NET<i></i></a></p><h1>必要的准备工作</h1><ul><li>AndroidStudio、Gradle和自己的开源项目这个必须有。</li><li>Jcenter是Bintray下的一个仓库，所以Bintray帐号必须的，没有的同学看下文如何申请。</li><li>网络必须是畅通的，要能访问<a target="_blank" rel="nofollow" href="https://link.zhihu.com/?target=https%3A//bintray.com/">Download Center Automation &amp; Distribution w. Private Repositories<i></i></a></li></ul><h2>如何申请Bintray帐号</h2><p>如果申请bintray账号很简单，这里就不细说了。申请好账号后，我们要拿到apikey，怎么拿到apikey，看下面操作，</p><p>第一步：登陆后点击你的头像，再点击Edit Profile 出现下面的界面（晴雨荡气为我的ID），<br></p><img title="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" alt="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" width="1188" src="https://pic2.zhimg.com/v2-97986d6ba61a78084d1ed2aa46db0f11_b.png" data-rawwidth="1188" data-rawheight="612" data-original="https://pic2.zhimg.com/v2-97986d6ba61a78084d1ed2aa46db0f11_r.png"><p>第二步：在输入框中输入你的密码Confirm Password。（我的密码张小月都懂）<img title="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" alt="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" width="1235" src="https://pic4.zhimg.com/v2-f2e1a3cc33fd968381818aa9c36fc857_b.png" data-rawwidth="1235" data-rawheight="657" data-original="https://pic4.zhimg.com/v2-f2e1a3cc33fd968381818aa9c36fc857_r.png">这里显示*******************就是我的api key ,我们先保存起来，到时候会用到。</p><h1>配置项目gradle和local.properties</h1><p>　　我们上传项目到Jcenter时使用Gradle的task自动完成，所以只需要配置好gradle一句话就可以完成上传了，下面是项目中的配置。 <br>　　正式开始之前我们上一张图，这里以an-aw-base2.0.2为例：</p><img title="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" alt="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" width="510" src="https://pic4.zhimg.com/v2-e706909c0cbd311f8ea193f702342e97_b.png" data-rawwidth="510" data-rawheight="800" data-original="https://pic4.zhimg.com/v2-e706909c0cbd311f8ea193f702342e97_r.png"><br><h2>（一）配置项目的gradle文件</h2><p>　　我们项目一般会有多个gradle配置文件，第一步要配置的是项目的gradle，而不是module/library的gradle，也就是上图[项目的gradle]标注的文件，把项目的内容改为如下内容即可。如果细心的同学会发现正杰前辈的dendent采用的android-maven-gradlepplugin的补丁的版本是1.3，而我这里修改成为了1.5我为什么知道是1.5了呢，具体可以参考这个网站，如果对方做了改变你们也要懂得去改变补丁的版本，另一个不用说你也知道了。</p><p>dcendents:<a target="_blank" rel="nofollow" href="https://link.zhihu.com/?target=https%3A//github.com/dcendents/android-maven-gradle-plugin">dcendents/android-maven-gradle-plugin<i></i></a></p><div><pre><code><span></span>buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.2.3'
        // jitpack发布github
        classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
        // 添加下面两行(y)代码即可。
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.4'
    }
}

allprojects {
    repositories {
        jcenter()
    }
    tasks.withType(JavaCompile) {
        configure(options) {
            incremental = true
        }
//        options.compilerArgs &lt;&lt; "-Xlint:unchecked" &lt;&lt; "-Xlint:deprecation"
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
</code></pre></div><p>No service of type Factory available in ProjectScopeServices.错误解决： <br><a target="_blank" rel="nofollow" href="https://link.zhihu.com/?target=https%3A//code.google.com/p/android/issues/detail%3Fid%3D219692"><span>https://</span><span>code.google.com/p/andro</span><span>id/issues/detail?id=219692</span><span></span><i></i></a></p><p><strong>特别注意</strong>，如果你的AndroidStuio更新到了2.1.3，也就是你引用的Gradle插件是2.1.3：</p><div><pre><code><span></span>classpath 'com.android.tools.build:gradle:2.1.3'
</code></pre></div><h2>（二）配置要上传的library/module的gradle文件</h2><p>　　第二步要配置的是要上传的module的gradle，而不是整个项目的gradle，也就是上图[library的gradle]标注的文件： 这个文件代码有点多，</p><div><pre><code><span></span>apply plugin: 'com.android.library'
//配置jitpack
/*apply plugin: 'com.github.dcendents.android-maven'
group = 'com.github.qydq'*/

//配置bintray(笨的方法)
//ext {
//    PUBLISH_GROUP_ID = 'cn.android.sunst'
//    PUBLISH_ARTIFACT_ID = 'an-base'
//    PUBLISH_VERSION = '2.0.1'
//}

apply plugin: 'com.github.dcendents.android-maven'
apply plugin: 'com.jfrog.bintray'

android {
    compileSdkVersion 25
    buildToolsVersion "24.0.3"

    defaultConfig {
        minSdkVersion 19
        targetSdkVersion 25
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"

    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
    useLibrary 'org.apache.http.legacy'
    dataBinding {
        enabled = true
    }
    sourceSets { main { res.srcDirs = ['src/main/res', 'src/main/res/color'] } }

    lintOptions {
        abortOnError false
//        disable 'TypographyFractions', 'TypographyQuotes'
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
        exclude group: 'com.android.support', module: 'support-annotations'
    })
    compile 'com.android.support:appcompat-v7:25.1.1'
    testCompile 'junit:junit:4.12'
    compile 'org.xutils:xutils:3.4.0'
    compile 'com.google.code.gson:gson:2.8.0'
    compile 'com.android.support:design:25.1.1'
    compile 'com.android.support:recyclerview-v7:25.1.1'
    compile 'com.nineoldandroids:library:2.4.0'
}

version = "2.0.2"

// 定义两个链接，下面会用到。
def siteUrl = 'https://github.com/qydq/an-aw-base' // 项目主页。
def gitUrl = 'git@github.com:qydq/an-aw-base.git' // Git仓库的url。
// 唯一包名，比如compile 'com.qydq:an-aw-base:1.0.1'中的com.qydq就是这里配置的。
group = "com.qydq"
install {
    repositories.mavenInstaller {
        // 生成pom.xml和参数
        pom {
            project {
                packaging 'aar'
                // 项目描述，复制我的话，这里需要修改。
                name 'AndServer For Android'// 可选，项目名称。
                description 'The Android build the framework of the Http server.'// 可选，项目描述。
                url siteUrl // 项目主页，这里是引用上面定义好。

                // 软件开源协议，现在一般都是Apache License2.0吧，复制我的，这里不需要修改。
                licenses {
                    license {
                        name 'The Apache Software License, Version 2.0'
                        url 'http://www.apache.org/licenses/LICENSE-2.0.txt'
                    }
                }

                //填写开发者基本信息，复制我的，这里需要修改。
                developers {
                    developer {
                        id 'qydq' // 开发者的id。
                        name 'qydq' // 开发者名字。
                        email 'qyddai@gmail.com' // 开发者邮箱。
                    }
                }

                // SCM，复制我的，这里不需要修改。
                scm {
                    connection gitUrl // Git仓库地址。
                    developerConnection gitUrl // Git仓库地址。
                    url siteUrl // 项目主页。
                }
            }
        }
    }
}

/*---build.gradle以下内容新增*/
// 指定编码
tasks.withType(JavaCompile) {
    options.encoding = "UTF-8"
}
// 打包源码
task sourcesJar(type: Jar) {
    from android.sourceSets.main.java.srcDirs
    classifier = 'sources'
}
task javadoc(type: Javadoc) {
    failOnError false
    source = android.sourceSets.main.java.sourceFiles
    classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
    classpath += configurations.compile
}
// 制作文档(Javadoc)
task javadocJar(type: Jar, dependsOn: javadoc) {
    classifier = 'javadoc'
    from javadoc.destinationDir
}
artifacts {
    archives sourcesJar
    archives javadocJar
}


// 这里是读取Bintray相关的信息，我们上传项目到github上的时候会把gradle文件传上去，所以不要把帐号密码的信息直接写在这里，写在local.properties中，这里动态读取。
Properties properties = new Properties()
properties.load(project.rootProject.file('local.properties').newDataInputStream())
bintray {
    user = properties.getProperty("bintray.user") // Bintray的用户名。
    key = properties.getProperty("bintray.apikey") // Bintray刚才保存的ApiKey。

    configurations = ['archives']
    pkg {
        repo = "maven"  // 上传到maven库。（这里要特别注意，如果写了maven报404错误，请在bintray创建一个仓库，这里填改成你创建的仓库的名字，如何创建请看下图。）
        name = "an"  // 发布到Bintray上的项目名字，这里的名字不是compile 'cn.android.sunst:an-base:2.0.3'中的aw。
        userOrg = 'bintray_user' // Bintray的用户名，2016年11月更新。
        websiteUrl = siteUrl
        vcsUrl = gitUrl
        licenses = ["Apache-2.0"]
        publish = true // 是否是公开项目。
    }
}
//apply from: 'https://raw.githubusercontent.com/blundell/release-android-library/master/android-release-aar.gradle'
//apply from: 'https://raw.githubusercontent.com/bingoogolapple/PublishAar/master/central-publish.gradle'
</code></pre></div><p>上面有两个特别的地方要注意：</p><ol><li>我们上传的开源项目一般会托管到github上，我们上传的时候会把项目和module的gradle文件传上去，所以不要把帐号密码的信息直接写在gradle文件中，而我们的local.properties文件一般不会上传（没经验的人可能会传），所以我们把用户隐私信息配置到local.properties， 在gradle中动态读取，如何读取，在上面最后一个代码块中有介绍。</li><li>我们看到在module/library的gradle中pkg下的name="an"，这个名字是我们项目在Bintray中的名字，我们依赖的gradle时，<div><pre><code><span></span>compile 'cn.android.sunst:an-base:2.0.3'
</code></pre></div>，那么compile中的an-base哪里配置呢？<strong>compile的library名称就是library/module的名称</strong>，如下图所示：</li></ol><img title="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" alt="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" width="1104" src="https://pic3.zhimg.com/v2-f5ae230ea0687360a4496dcafadfc90a_b.png" data-rawwidth="1104" data-rawheight="769" data-original="https://pic3.zhimg.com/v2-f5ae230ea0687360a4496dcafadfc90a_r.png"><br><img title="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" alt="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" width="514" src="https://pic4.zhimg.com/v2-1b34d583330408de470a7a614885e807_b.png" data-rawwidth="514" data-rawheight="542" data-original="https://pic4.zhimg.com/v2-1b34d583330408de470a7a614885e807_r.png"><h2>（三）在local.properties中为module/libraray配置用户隐私信息</h2><p>　　我们会在local.properties中配置很多变量，别的地方动态引用或者读取，这样就可以做到修改一个地方，其它地方都可以不用改了：</p><div><pre><code><span></span>ndk.dir=D\:\\ide\\android-sdk\\ndk-bundle
sdk.dir=D\:\\ide\\android-sdk
bintray.user=qyddai@gmail.com
bintray.apikey=fa************************5a
</code></pre></div><h1>上传项目到Jcenter</h1><p>　　准备工作都做完啦，最后一步就是上传操作了，点击AndroidStudio底部的Terminal，观察下Terminal显示的路径是否是你当前项目的root。</p><ol><li>这里如果你系统配置了gradle的用户环境，输入gradle install，如果没有配置gradle用户环境，输入gradlew install，如果没有问题，最终你会看到BUILD SUCCESSFUL。</li><li>如果你看到了生成javadoc时编译不过，那么要看下在gradle中task javadoc下有没有failOnError false这句话，在刚才编写gradle时提示过了。如果加了这句而你的javadoc写的不规范会有警告，你不用鸟它。</li><li>最后一步，运行gradle install后看到BUILD SUCCESSFUL后，再输入上传命令gradle bintrayUpload，等一分钟左右就执行完了，会提示SUCCESSFUL。如图。</li><li>浏览器<a target="_blank" rel="nofollow" href="https://link.zhihu.com/?target=https%3A//bintray.com/">Download Center Automation &amp; Distribution w. Private Repositories<i></i></a>后会看到你的项目。</li></ol><p><img title="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" alt="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" width="818" src="https://pic2.zhimg.com/v2-2715dd3357fd263f5b58c62b49828e0d_b.png" data-rawwidth="818" data-rawheight="375" data-original="https://pic2.zhimg.com/v2-2715dd3357fd263f5b58c62b49828e0d_r.png">传完成咯，别着急喔，我们项目上传完成后还需要Bintray的管理员审核，所以在刚才项目页面点击进去查看详情，点击Add to Jcetner：只有等审核后我们的项目才能Gradle 依赖<br></p><p><img title="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" alt="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" width="880" src="https://pic1.zhimg.com/v2-c411e67f56582dd9ea07631e3008cf60_b.png" data-rawwidth="880" data-rawheight="558" data-original="https://pic1.zhimg.com/v2-c411e67f56582dd9ea07631e3008cf60_r.png">这里有坑，请注意区分bintray中的名称和项目中的名称。</p><p>审核差不多一天的时间，你就会在bintray中收到消息，如图</p><img title="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" alt="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" width="1388" src="https://pic2.zhimg.com/v2-1276d69b8c7fed3f2efc4d7ff1a4b605_b.png" data-rawwidth="1388" data-rawheight="646" data-original="https://pic2.zhimg.com/v2-1276d69b8c7fed3f2efc4d7ff1a4b605_r.png"><br><p>在你的邮件中也可以收到推送的信息，Your request to include your package /has been approved。意思就是说，你的请求什么什么的，框架an-aw-base2.0.2可以应用了。如图。</p><img title="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" alt="android studio 发布开源项目到jcenter/bintray. - Nakama - Bgwan" width="1600" src="https://pic4.zhimg.com/v2-cd30981332c5a567ecec9a937f0daf2b_b.png" data-rawwidth="1600" data-rawheight="340" data-original="https://pic4.zhimg.com/v2-cd30981332c5a567ecec9a937f0daf2b_r.png"><br><br><p>题外话，明天就是3月28号了，28号啊。张小月啊，张小月啊，……，这篇日志发布到yTips专栏。</p></div><div><br></div><div><wbr></div>