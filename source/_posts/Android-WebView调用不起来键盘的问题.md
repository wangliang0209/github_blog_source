---
title: Android WebView调用不起来键盘的问题
date: 2017-03-18 13:50:09
tags:
---
今天遇到webview中input标签点击弹不出键盘，找了半天没有答案。终于在一个不起眼的地方发现了问题。

首先试验过的方法
  > requestFoucs();无效。  
  > requestFoucsFromTouch();无效。  
  > webview.setTouchListener；无效。
  
##### 问题所在  
继承WebView时，注意构造方法：
```
public CommonWebView(Context context) {
	super(context);
	init();
}
public CommonWebView(Context context, AttributeSet attrs) {
	super(context, attrs);
	init();
}
public CommonWebView(Context context, AttributeSet attrs, int defStyleAttr) {
	super(context, attrs, defStyleAttr);
	init();
}```

错误在于 defStyleAttr不能传0，如下错误写法：
```
public CommonWebView(Context context) {  
	this(context,null,0);
}
public CommonWebView(Context context, AttributeSet attrs) {
	this(context, attrs,0);
}
public CommonWebView(Context context, AttributeSet attrs, int defStyleAttr) {
	super(context, attrs, defStyleAttr);
	init();
}
```