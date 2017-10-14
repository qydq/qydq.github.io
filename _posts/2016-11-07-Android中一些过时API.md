---
title:  "Android中一些过时API"
date:   2016-11-07 14:30
categories: _posts
---

迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/23930101620161075343107/">《Android中一些过时API》</a>

Spanned result = Html.fromHtml(mNews.getTitle());
...
...
mNewsTitle.setText(result);

@SuppressWarnings("deprecation")
   public static Spanned fromHtml(String source) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
            return Html.fromHtml(source, Html.FROM_HTML_MODE_LEGACY);
        } else {
            return Html.fromHtml(source);
        }
    }

1.TextView

<TextView
    android:id="@+id/user_root"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:clickable="true"
    android:autoLink="web"
    android:textColor="@color/lanse" />

mUserRoot = (TextView) findViewById(R.id.user_root);
mUserRoot.setMovementMethod(LinkMovementMethod.getInstance());
String html = "<a href=\"http://www.miracase.com/brand.asp\">iBreezee用户协议</a>";
mUserRoot.setText(Html.fromHtml(html));//过时
mUserRoot.setMovementMethod(LinkMovementMethod.getInstance());

//-------------
Html.fromHtml deprecated in Android N




2)textColor

// 同时兼容高、低版本
ContextCompat.getColor(Context context, int id);

 public Notification setNotification(String showTitle, String showInfo, Intent intent, boolean isCancel) {
        nm = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
        NotificationCompat.Builder builder = new NotificationCompat.Builder(getApplicationContext());//v7就ok
//        builder = new Notification.Builder(getApplicationContext());//v4就ok
        int smallIconId = R.drawable.base_drawable_circle_click;
//        Bitmap largeIcon = ((BitmapDrawable) getResources().getDrawable(R.drawable.ic_launcher)).getBitmap();//过时的解决方法。
        Drawable drawable = ContextCompat.getDrawable(getApplicationContext(), R.drawable.ic_launcher);
        BitmapDrawable bitmapDrawable = (BitmapDrawable) drawable;
        Bitmap largeIcon = bitmapDrawable.getBitmap();
        builder.setLargeIcon(largeIcon)
                .setSmallIcon(smallIconId)
                .setContentTitle(showTitle)
                .setContentText(showInfo)
                .setTicker(showTitle)
                .setAutoCancel(isCancel)
                .setContentIntent(PendingIntent.getActivity(getApplicationContext(), 0, intent, 0));
        Notification n = builder.build();
        nm.notify(NOTIFICATION_START, n);
        return n;
    }