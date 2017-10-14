---
title:  "ListView 选中item高亮或改变颜色。"
date:   2016-12-01 14:30
categories: _posts
---

迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/239301016201611134331730/">《ListView 选中item高亮或改变颜色。》</a>

mSlidingDeleteLv.setOnItemClickListener(new AdapterView.OnItemClickListener() {

            @Override
            public void onItemClick(AdapterView<?> view, View parent, int position,
                                    long id) {
                Toast.makeText(TestActivity.this, "click item " + position + "name:" + arrays.get(position) + "pass:" + arrayPass.get(position), Toast.LENGTH_SHORT).show();
                String recentlyChooseName = arrays.get(position);
                editText1.setText(recentlyChooseName);
                editText2.setText(arrayPass.get(position));

                edit.putString("recentlyChooseName", recentlyChooseName);
                edit.putInt("mPosition", position);
                edit.commit();

//                mSlidingDeleteLv.setItemChecked(position, true);//设置某行点击了。
                mSlidingDeleteLv.setSelection(position);//设置某行选中了；
//                mAdapter.notifyDataSetInvalidated();
                mAdapter.notifyDataSetChanged();//通知改变adapter
madapter.setSelectItem(postion);
            }
        });
public  void setSelectItem(int selectItem) {
    this.selectItem = selectItem;
  }
  private int  selectItem=-1;
if (position == recentlyChoosePosition) {
    holder.iv_choose_wifi.setVisibility(View.VISIBLE);
} else {
    holder.iv_choose_wifi.setVisibility(View.INVISIBLE);
}
return convertView;


第二种方法：
siteListView.setOnItemClickListener(new OnItemClickListener() {
    @Override
     public void onItemClick(AdapterView<?> parent, View view,int position, long id) {
         for(int i=0;i<parent.getCount();i++){
             View v=parent.getChildAt(parent.getCount()-1-i);
             if (position == i) {
                 v.setBackgroundColor(Color.RED);
             } else {
                 v.setBackgroundColor(Color.TRANSPARENT);
             }
         }
     }
 });

备注：
listview.setSelector(R.drawable.nocolor);图片为一张无色透明图片即可，或者
android:listSelector="@drawable/nocolor"