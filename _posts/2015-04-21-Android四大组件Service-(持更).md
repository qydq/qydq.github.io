---
title:  "Android四大组件Service-(持更)"
date:   2015-04-21 14:30
categories: _posts
---

迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/239301016201532123742725/">《bound service 和start Service》</a>
迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/239301016201532112832625/">《Service组件简单介绍  》</a>

2017年备注： 该日志需要重新更新整理。

Service是什么？
Service不是什么？
Service的声明周期
Service如何启动？

1.Service是一个应用程序组件
Service没有图形化界面
Service通常用来处理一些耗时比较长的操作（下载，播放MP3等等）
可以使用Service更新ContentProvider,发送Intent以及启动系统的通知（更新UI）

2.Service不是一个单独的进程，Service不是一个线程。
进程 和 线程的区别（一个进程可以有多个线程，一个应用程序至少有一个进程）线程不直接占用系统的资源，主要是占用进程的资源。
a service is not a separate process ,a service is not a thread

3.FirstService extends Servie {

IBinder onBind(Intent intent){return null;}
void onCreate(){}
int onStartCommand(Intent intent ,int flas,int startId){}
return START_NOT_STICLY;}
void onDestory(){super.onDestroy();}

TestAcitviy extends Activiy {
class StartServiceListener implements OnclickListener{
void onClick(View v){
Intent intent = new Intent();
intent.setClass(TestAcitivy.this,FirstService.class);
startService(intent);
}
class StopServiceListener implements OnclickListener{
void onClick(View v){
Intent intent = new Intent();
instent.setClass(TestActivity.this,Firstservice.class);
stopService(intent);
}
}
}
卸载程序
adb uninstall 名称



bound service 和start Service
1.什么是bound Service
2.Bound Service 和 Start Service的区别
Start Service 是属于比较简单的启动，Bound Service主要是让

接受的数据再一次到activity组件中、或者是其他组件中。

这两种相当与客服端和服务端的模式，Bound就是服务端，那么类

似的activity就是客户端了。
a bound service offers a client=servier interface that

allows components to interact with the service ,send

requests ,get results,and even do so across processes with

interprocess communication.
Activiy就可以接受到Bound Service当中的一些情况。
这也就是 bound Service带来的价值。

3.怎样绑定
SecondService.java

public class SecondService extends Service{

 @Override
 //当其他的应用程序组件（主要指Activiy）绑定到当前

的Service,调用这个方法
 public IBinder onBind(Intent intent) {
  // TODO Auto-generated method stub
  IBinder binder = new FirstBinder();
  return binder;
 }
 class FirstBinder extends Binder{
  public String getData(){
   return "test data ,nakama";
  }
 }

}

MainActivity.java

public class MainActivity extends Activity {

    Button button = null;
 @Override
 protected void onCreate(Bundle savedInstanceState)

{
  super.onCreate(savedInstanceState);
  setContentView(R.layout.activity_main);
  button = (Button)findViewById

(R.id.button);

  button.setOnClickListener(new

OnClickListener() {

   @Override
   public void onClick(View v) {
    Intent intent = new

Intent();
    intent.setClass

(MainActivity.this, SecondService.class);
    // TODO Auto-generated

method stub
    bindService(intent,

connection, BIND_AUTO_CREATE);
   }
  });
 }
 ServiceConnection connection = new

ServiceConnection() {

  @Override
  public void onServiceDisconnected

(ComponentName name) {
   // TODO Auto-generated method stub

  }

  @Override
  public void onServiceConnected

(ComponentName name, IBinder service) {
   // TODO Auto-generated method stub
   FirstBinder binder =

(FirstBinder)service;
   String dataString =

binder.getData();
   System.out.println("data ---

>:shi"+dataString);
  }
 };
}

解释：bound Service binder对象传入MainActivity中（binder对

象包含了传递过来的数据）、然后在bindService

(intent,connection,BIND_AUTO_CREATE);建立service和activity

之间的联系，需要传入三个参数。
connection链接对象设计一个匿名内部类 创建了connection对象

，需要复写里面的两个方法、onServiceConnected

(ComponentName,IBinder service){中，IBinder 就是binder对象

从bound Service 中传递过来的参数}