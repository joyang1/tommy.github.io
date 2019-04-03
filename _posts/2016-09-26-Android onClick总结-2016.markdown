---
layout:     post
title:      "Android Button onClick事件总结"
subtitle:   " \"Button onClick事件\""
date:       2016-09-26 12:41:00
author:     "Tommy"
header-img: "img/post-bg-android.jpg"
catalog: true
tags:
    - Android
    - Android控件
    - Android Button
---

> “Let's Start. ”

Android Button点击事件以及响应的四种方式,下面我们分别介绍一下四种方式的使用。


## First 匿名内部类

button1.setOnClickListener(new Button.OnClickListener() {<br/>
	@Override<br/>
	public void onClick(View v) {<br/>
		display.setText("现在使用的是匿名内部类的方式在实现button点击事件的响应");<br/>
	}<br/>
});<br/>


## Second 自定义点击事件监听类

在onCreate方法里面加上<br/>
button2.setOnClickListener(new MyListener());

private class MyListener implements View.OnClickListener{<br/>
	@Override<br/>
	public void onClick(View v) {<br/>
		if(v.getId() == R.id.button2){<br/>
			display.setText("现在使用的是自定义点击事件监听类来实现button点击事件的响应");<br/>
		}<br/>
	}<br/>
}<br/>

## Third 实现View.OnClickListener接口

在我们Activity类上实现View.OnClickListener接口

在onCreate方法里面加上
button3.setOnClickListener(this);

@Override<br/>
public void onClick(View v) {<br/>
	display.setText("现在是使用实现View.OnClickListener接口的方式实现button点击事件的响应");<br/>
}<br/>

## Fourth xml布局文件指定按钮的onClick属性

在xml布局文件指定按钮的onClick属性，设置值为:button4,实现该方法如下

public void button4(View v){<br/>
	display.setText("现在是使用在xml布局文件指定按钮的onClick属性方式实现button点击事件的响应");<br/>
}<br/>

布局文件如下:
<div>
<TextView <br/>
            android:id="@+id/text"<br/>
            android:text="Button点击事件以及响应的四种方式"<br/>
            android:textSize="24sp"<br/>
            android:gravity="center_horizontal"<br/>
            android:layout_width="wrap_content"<br/>
            android:layout_height="wrap_content"/><br/><br/>

<TextView<br/>
		android:id="@+id/display"<br/>
		android:textSize="16sp"<br/>
		android:gravity="center_horizontal"<br/>
		android:layout_below="@id/text"<br/>
		android:layout_width="wrap_content"<br/>
		android:layout_height="wrap_content"/><br/><br/>
		
<Button<br/>
		android:id="@+id/button1"<br/>
		android:text="方式一"<br/>
		android:textSize="18sp"<br/>
		android:layout_below="@id/display"<br/>
		android:layout_width="wrap_content"<br/>
		android:layout_height="wrap_content"/><br/><br/>

<Button<br/>
		android:id="@+id/button2"<br/>
		android:text="方式二"<br/>
		android:textSize="18sp"<br/>
		android:layout_below="@id/button1"<br/>
		android:layout_width="wrap_content"<br/>
		android:layout_height="wrap_content"/><br/><br/>
		
<Button<br/>
		android:id="@+id/button3"<br/>
		android:text="方式三"<br/>
		android:textSize="18sp"<br/>
		android:layout_below="@id/button2"<br/>
		android:layout_width="wrap_content"<br/>
		android:layout_height="wrap_content"/><br/><br/>

<Button<br/>
		android:id="@+id/button4"<br/>
		android:text="方式四"<br/>
		android:textSize="18sp"<br/>
		android:layout_below="@id/button3"<br/>
		android:onClick="button4"<br/>
		android:layout_width="wrap_content"<br/>
		android:layout_height="wrap_content"/><br/><br/>
</div><br/>

<p>源码下载地址: <a href="https://github.com/joyang1/ButtonClickDemo/" target="_blank" title="ButtonDemo">查看</a> </p>
