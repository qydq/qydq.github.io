---
title:  "Android开发动画向左向右动画，TextView设置下划线，String[]数组转换，缓存历史纪录备份"
date:   2016-11-03 18:50:48
categories: _posts
---


迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/239301016201610353813977/">《Android开发动画向左向右动画，TextView设置下划线，String[]数组转换，缓存历史纪录备份》</a>

首先，这边文章给自己的一个备份，最近工作太久发现记忆力有所下降，所以做一个笔记，省得到处博客翻阅，

1，向左向右动画，


   llAccount.setAnimation(AnimationUtils.makeOutAnimation(this, false));//向左移出
    llVerify.setAnimation(AnimationUtils.makeInAnimation(this, false));//向左移入


     //animation
   llAccount.setAnimation(AnimationUtils.makeInAnimation(this, true));//向右移入
    llVerify.setAnimation(AnimationUtils.makeOutAnimation(this, true));//向右移出

2，TextView设置下划线可点击，网页

mUserRoot.setText(Html.fromHtml("<a href=\"http://www.miracase.com/brand.asp\">iBreezee用户协议</a>"));
---
tvNewAccount = (TextView) findViewById(R.id.tvNewAccount);
tvNewAccount.getPaint().setFlags(Paint.UNDERLINE_TEXT_FLAG);
3，String[]数值转化为ListView

if (resultCode == RESULT_OK) {
                    bundle = data.getExtras();
                    String listsString = bundle.getString("listsString");
                    if (!TextUtils.isEmpty(listsString)) {
                        try {
                            if (listsString.contains("[")) {
                                String subString = listsString.substring(1, listsString.length() - 1);
                                String[] listArray = subString.replace(" ", "").split(",");
//                List<String> lists = Arrays.asList(listArray);//也可以，这里有space的坑。
//                System.out.println(getClass().getName() + "---qydq--测试lists--" + lists.toString());
                                StringBuffer codesBuffer = new StringBuffer();
                                for (String list : listArray) {
                                    String code = list.substring(list.length() - 12, list.length());
                                    codesBuffer.append(code + "@");
                                }
                                code = codesBuffer.toString().substring(0, codesBuffer.length() - 1);
                            } else {
                                Toast.makeText(x.app(), "没有[" + listsString, Toast.LENGTH_LONG).show();
                            }
                            System.out.println(getClass().getName() + "---qydq--测试code--" + code);
                        } catch (Exception e) {
                            e.printStackTrace();
                        }
                    }





4，缓存历史数据，like用户名，密码，啥的。

1）存

mHistoryKeywords = new ArrayList<String>();
String oldText = preferences.getString("username", "");
String oldPass = preferences.getString("password", "");
StringBuilder builder = new StringBuilder(username);
StringBuilder bdps = new StringBuilder(password);
builder.append("," + oldText);
bdps.append("," + oldPass);
if (!TextUtils.isEmpty(username) && !oldText.contains(username + ",")) {
    SharedPreferences.Editor editor = preferences.edit();
    editor.putString("username", builder.toString());//username作为list会使用到
    editor.putString("password", bdps.toString());//username作为list会使用到
    editor.putString("account", username);
    editor.putString("pwd", password);
    mHistoryKeywords.add(0, username);
    editor.commit();
}
2）取

// 一个自定义的布局，作为显示记录的内容
public void showPopupWindow(View view) {
    View contentView = LayoutInflater.from(mContext).inflate(R.layout.sst_activity_login_popwind, null);
    ListView listView = (ListView) contentView.findViewById(R.id.lvLogin);
    String history = preferences.getString("username", "");
    String password = preferences.getString("password", "");
    final String[] mHistoryArr = history.split(",");
    final String[] passwordArr = password.split(",");
    ArrayAdapter mArrAdapter = new ArrayAdapter<String>(mContext, android.R.layout.simple_list_item_1, mHistoryArr);
    listView.setAdapter(mArrAdapter);
    mArrAdapter.notifyDataSetChanged();
    final PopupWindow popupWindow = new PopupWindow(contentView, ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT, true);
    popupWindow.setTouchable(true);
    popupWindow.setTouchInterceptor(new View.OnTouchListener() {
        @Override
        public boolean onTouch(View v, MotionEvent event) {
            Log.i(TAG, "onTouch : ");
            return false;
            // 这里如果返回true的话，touch事件将被拦截
            // 拦截后 PopupWindow的onTouchEvent不被调用，这样点击外部区域无法dismiss
        }
    });
    // 如果不设置PopupWindow的背景，无论是点击外部区域还是Back键都无法dismiss弹框
    // 我觉得这里是API的一个bug
    popupWindow.setBackgroundDrawable(getResources().getDrawable(R.drawable.wt_drawable_arrow_history));
    // 设置好参数之后再show
    popupWindow.showAsDropDown(view);
    listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
        @Override
        public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
            popupWindow.dismiss();
            if (mHistoryArr.length != 0) {
                editText1.setText(mHistoryArr[position]);
                editText2.setText(passwordArr[position]);
            }
        }
    });
}
阅读(8)| 评论(0)