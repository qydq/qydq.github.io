---
title:  "在activity中更新UI的方法。"
date:   2016-12-27 14:30
categories: _posts
---

迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/2393010162016112795144657/">《在activity中更新UI的方法。》</a>

该日志做为草稿日志，写在这儿便于下次使用参考，后续更新至知乎。
方法一。广播。
问题一：如何在外部广播中更新activity的内容呢？。
大家先看一下外部的广播。
if (intent.getAction().equals("DELIVERY_MESS_ACTION")) {
            if (getResultCode() == -1) {
                Toast.makeText(context, "发送成功", 1).show();
            } else if (getResultCode() == 0) {
                Toast.makeText(context, "发送失败", 1).show();
            }
        }
如果是想要在activity中，不用Toast怎么办呢。
下面看看方法。
Tips ：android中的消息传递机制，也就是Handler，不错，这个可行。还有那就是runOnUiThread 这个方法。

questions：如果直接使用，MainActivity.this.runOnUiThread 会导致""No enclosing instance of the type MainActivity is accessible in scope" " 个问题，这是因为MainActivity 并不是内部类，怎么解决呢？我们现在想要在广播中得到activity的上下文，这是这个问题的关键！，由于activity不是我们new出来的，所以可以这么做
在activity中创建一个静态类成员变量 instance
在onCreate方法的最后，将this赋值给instance
自己写一个静态方法getInstance
在广播中得到activity的上下文即可
这样以后，我们可以参考以下代码实现。

if (intent.getAction().equals("DELIVERY_MESS_ACTION")) {
            if (getResultCode() == -1) {
                // Toast.makeText(context, "发送成功", 1).show();
                MessSendActivity.getInstance().runOnUiThread(new Runnable() {

                    @Override
                    public void run() {
                        // TODO Auto-generated method stub
                        // Toast.makeText(MessSendActivity.getInstance(),
                        // "from messSend", 1).show();
                        ProgressBar pb = (ProgressBar) MessSendActivity
                                .getInstance().findViewById(R.id.pb);
                        pb.setVisibility(View.INVISIBLE);
                    }
                });
            } else if (getResultCode() == 0) {
                Toast.makeText(context, "发送失败", 1).show();
            }
        }

问题二：如何用内部类的广播呢。
参考：

public class MainActivity extends Activity {

    private MyReceiver receiver;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        receiver = new MyReceiver(new Handler()); // Create the receiver
        registerReceiver(receiver, new IntentFilter("some.action")); // Register receiver

        sendBroadcast(new Intent("some.action")); // Send an example Intent
    }

    public static class MyReceiver extends BroadcastReceiver {

        private final Handler handler; // Handler used to execute code on the UI thread

        public MyReceiver(Handler handler) {
            this.handler = handler;
        }

        @Override
        public void onReceive(final Context context, Intent intent) {
            // Post the UI updating code to our Handler
            handler.post(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(context, "Toast from broadcast receiver", Toast.LENGTH_SHORT).show();
                }
            });
        }
    }
}
问题三，内部广播静态注册注意事项？

Mainfest.xml参考。
<receiver android:name=".MainActivity$DataAcceptBroadcastReceiver">
    <intent-filter>
        <action android:name="accept" />
    </intent-filter>
</receiver>
注意事项：内部类的广播一定是static的，不然会找不到。内部类使用$符号而不是.，使用会出现类找不到异常。
public static class DataAcceptBroadcastReceiver extends BroadcastReceiver {
    private final Handler handler;

    public DataAcceptBroadcastReceiver() {
        handler = mHandler;
    }

    public DataAcceptBroadcastReceiver(Handler handler) {
        this.handler = handler;
    }

    @Override
    public void onReceive(Context context, final Intent intent) {
        System.out.println("--qydq--MainActivity收到DataSendClientHandler消息--内容--" + intent.getStringExtra("ret"));
        isRunnint = true;
        acceptAccount++;
        Message msg = new Message();
        msg.obj = intent.getStringExtra("ret");
        msg.what = SERVERDATA;
        handler.sendMessage(msg);
    }
用handler去更新UI，可以TextView等可以是静态的。也可以是private TextView tvTime类型的。如果是上面引用的区别为：
@@@@@new DataAcceptBroadcastReceiver (mHandler);这种是在外部的，mHandler是Activity的成员静态变量。
//mHandler用于更新UI。
public static final Handler mHandler = new Handler() {
    @Override
    public void handleMessage(Message msg) {
        super.handleMessage(msg);
        switch (msg.what)
private static TextView tvTime;
private static TextView tvContent;
private static TextView tvBtnMes;
private static TextView tvContentTips;

@@@@@new DataAcceptBroadcastReceiver (new Handler);而这种方式直接在onReceiver方法中使用
 handler.post(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(context, "Toast from broadcast receiver", Toast.LENGTH_SHORT).show();
                }
            });
在使用的时候，程序内注册，register是放在onCreate中，unregister是放在onDestroy中。

方法二：可以参考，直接静态的控件，生命为public,然后，(MainActivity)context.textview.setText();
GitHub.com/qydq/an-aw/
GitHub.com/qydq/aw-an/
项目

public class SlideBackActivity extends Activity{

     //手指上下滑动时的最小速度
     private static final int YSPEED_MIN = 1000;

     //手指向右滑动时的最小距离
     private static final int XDISTANCE_MIN = 50;

     //手指向上滑或下滑时的最小距离
     private static final int YDISTANCE_MIN = 100;

     //记录手指按下时的横坐标。
     private float xDown;

     //记录手指按下时的纵坐标。
     private float yDown;

     //记录手指移动时的横坐标。
     private float xMove;

     //记录手指移动时的纵坐标。
     private float yMove;

     //用于计算手指滑动的速度。
     private VelocityTracker mVelocityTracker;

     @Override
     public boolean dispatchTouchEvent(MotionEvent event) {
          createVelocityTracker(event);
          switch (event.getAction()) {
          case MotionEvent.ACTION_DOWN:
                xDown = event.getRawX();
                yDown = event.getRawY();
                break;
          case MotionEvent.ACTION_MOVE:
                xMove = event.getRawX();
                yMove= event.getRawY();
                //滑动的距离
                int distanceX = (int) (xMove - xDown);
                int distanceY= (int) (yMove - yDown);
                //获取顺时速度
                int ySpeed = getScrollVelocity();
                //关闭Activity需满足以下条件：
                //1.x轴滑动的距离>XDISTANCE_MIN
                //2.y轴滑动的距离在YDISTANCE_MIN范围内
                //3.y轴上（即上下滑动的速度）<XSPEED_MIN，如果大于，则认为用户意图是在上下滑动而非左滑结束Activity
                if(distanceX > XDISTANCE_MIN &&(distanceY<YDISTANCE_MIN&&distanceY>-YDISTANCE_MIN)&& ySpeed < YSPEED_MIN) {
                      finish();
                }
                break;
          case MotionEvent.ACTION_UP:
                recycleVelocityTracker();
                break;
          default:
                break;
          }
          return super.dispatchTouchEvent(event);
    }

    /**
      * 创建VelocityTracker对象，并将触摸界面的滑动事件加入到VelocityTracker当中。
      *
      * @param event
      *
      */
    private void createVelocityTracker(MotionEvent event) {
         if (mVelocityTracker == null) {
                mVelocityTracker = VelocityTracker.obtain();
         }
         mVelocityTracker.addMovement(event);
    }

    /**
      * 回收VelocityTracker对象。
      */
    private void recycleVelocityTracker() {
          mVelocityTracker.recycle();
          mVelocityTracker = null;
    }

    /**
      *
      * @return 滑动速度，以每秒钟移动了多少像素值为单位。
      */
    private int getScrollVelocity() {
          mVelocityTracker.computeCurrentVelocity(1000);
          int velocity = (int) mVelocityTracker.getYVelocity();
          return Math.abs(velocity);
    }

}


public class SlideBackActivity extends Activity{

     //手指上下滑动时的最小速度
     private static final int YSPEED_MIN = 1000;

     //手指向右滑动时的最小距离
     private static final int XDISTANCE_MIN = 50;

     //手指向上滑或下滑时的最小距离
     private static final int YDISTANCE_MIN = 100;

     //记录手指按下时的横坐标。
     private float xDown;

     //记录手指按下时的纵坐标。
     private float yDown;

     //记录手指移动时的横坐标。
     private float xMove;

     //记录手指移动时的纵坐标。
     private float yMove;

     //用于计算手指滑动的速度。
     private VelocityTracker mVelocityTracker;

     @Override
     public boolean dispatchTouchEvent(MotionEvent event) {
          createVelocityTracker(event);
          switch (event.getAction()) {
          case MotionEvent.ACTION_DOWN:
                xDown = event.getRawX();
                yDown = event.getRawY();
                break;
          case MotionEvent.ACTION_MOVE:
                xMove = event.getRawX();
                yMove= event.getRawY();
                //滑动的距离
                int distanceX = (int) (xMove - xDown);
                int distanceY= (int) (yMove - yDown);
                //获取顺时速度
                int ySpeed = getScrollVelocity();
                //关闭Activity需满足以下条件：
                //1.x轴滑动的距离>XDISTANCE_MIN
                //2.y轴滑动的距离在YDISTANCE_MIN范围内
                //3.y轴上（即上下滑动的速度）<XSPEED_MIN，如果大于，则认为用户意图是在上下滑动而非左滑结束Activity
                if(distanceX > XDISTANCE_MIN &&(distanceY<YDISTANCE_MIN&&distanceY>-YDISTANCE_MIN)&& ySpeed < YSPEED_MIN) {
                      finish();
                }
                break;
          case MotionEvent.ACTION_UP:
                recycleVelocityTracker();
                break;
          default:
                break;
          }
          return super.dispatchTouchEvent(event);
    }

    /**
      * 创建VelocityTracker对象，并将触摸界面的滑动事件加入到VelocityTracker当中。
      *
      * @param event
      *
      */
    private void createVelocityTracker(MotionEvent event) {
         if (mVelocityTracker == null) {
                mVelocityTracker = VelocityTracker.obtain();
         }
         mVelocityTracker.addMovement(event);
    }
    /**
      * 回收VelocityTracker对象。
      */
    private void recycleVelocityTracker() {
          mVelocityTracker.recycle();
          mVelocityTracker = null;
    }
    /**
      *
      * @return 滑动速度，以每秒钟移动了多少像素值为单位。
      */
    private int getScrollVelocity() {
          mVelocityTracker.computeCurrentVelocity(1000);
          int velocity = (int) mVelocityTracker.getYVelocity();
          return Math.abs(velocity);
    }

}
阅读(31)| 评论(1)