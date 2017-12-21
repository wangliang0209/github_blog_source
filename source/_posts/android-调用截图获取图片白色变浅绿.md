---
title: android 调用截图获取图片白色变浅绿
date: 2017-12-21 14:56:40
tags:
---
这两天做涂鸦的题目，实现方案为从网上加载一张图，然后再图片上蒙一层透明层，在上面涂鸦，涂鸦完成之后，通过截图保存图和涂鸦到一张图上，达到所画即所得的效果。

但是实现后遇到了这个坑，花了一天时间终于找到原因。
首先看我的实现
布局文件 
```
 <RelativeLayout android:id="@+id/doodle_layout"  
            android:layout_width="match_parent"  
            android:layout_height="wrap_content"
            android:layout_centerVertical="true">
            <ImageView android:id="@+id/write_iv_img"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:background="#FFFFFF"
                android:adjustViewBounds="true"  //达到图片match_parent效果
                />
            <com.wl.base.doodle.DoodleImageView
                android:id="@+id/doodle_paint"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_alignLeft="@id/write_iv_img"
                android:layout_alignRight="@id/write_iv_img"
                android:layout_alignTop="@id/write_iv_img"
                android:layout_alignBottom="@id/write_iv_img">
            </com.zuoyeben123.base.doodle.DoodleImageView>
        </RelativeLayout>
```

代码加载
> ImageLoader.getInstance().displayImage(mUrl, mIvImg);

截图代码
> Bitmap bm = Bitmap.createBitmap(mDoodleContainer.getWidth(), mDoodleContainer.getHeight(), Bitmap.Config.ARGB_8888);
        //使用Canvas，调用自定义view控件的onDraw方法，绘制图片
        Canvas canvas = new Canvas(bm);
        mDoodleContainer.draw(canvas);

其实和调用getDrawingCache效果一致。

按照上面的代码出来的效果就是白色的图片截出图来会发绿。

###问题解决

首先出现这个问题的原因是我用了ImageLoader  并且为了节省内存，display 我用的是`RGB_565`，问题就在这个RGB_565上面。

下面是解决办法：
> DisplayImageOptions options = new DisplayImageOptions.Builder()
            .cacheInMemory(true)
            .cacheOnDisk(true)
            .imageScaleType(ImageScaleType.IN_SAMPLE_INT)//设置图片以如何的编码方式显示
            .bitmapConfig(`Bitmap.Config.ARGB_8888`)
            .build();
ImageLoader.getInstance().displayImage(mUrl, mIvImg, options);

这样修改后，问题就解决了。出坑~~~