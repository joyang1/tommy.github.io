---
layout:     post
title:      "Android Button onClick事件总结"
subtitle:   " \"Button onClick事件\""
date:       2016-09-26 12:41:00
author:     "Tommy"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Android
    - Android控件
    - Android Button
---

> “Let's Start. ”

Android Button点击事件以及响应的四种方式,下面我们分别介绍一下四种方式的使用。


## First 匿名内部类

\button1.setOnClickListener(new Button.OnClickListener() {
	@Override
	public void onClick(View v) {
		display.setText("现在使用的是匿名内部类的方式在实现button点击事件的响应");
	}
\});


## Second 自定义点击事件监听类

\在onCreate方法里面加上
button2.setOnClickListener(new MyListener());

private class MyListener implements View.OnClickListener{
	@Override
	public void onClick(View v) {
		if(v.getId() == R.id.button2){
			display.setText("现在使用的是自定义点击事件监听类来实现button点击事件的响应");
		}
	}
\}

## Third 实现View.OnClickListener接口

在我们Activity类上实现View.OnClickListener接口

\在onCreate方法里面加上
button3.setOnClickListener(this);

@Override
public void onClick(View v) {
	display.setText("现在是使用实现View.OnClickListener接口的方式实现button点击事件的响应");
\}

## Fourth xml布局文件指定按钮的onClick属性

在xml布局文件指定按钮的onClick属性，设置值为:button4,实现该方法如下

\public void button4(View v){
	display.setText("现在是使用在xml布局文件指定按钮的onClick属性方式实现button点击事件的响应");
\}

布局文件如下:
\<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:paddingLeft="@dimen/activity_horizontal_margin"
        android:paddingRight="@dimen/activity_horizontal_margin"
        android:paddingTop="@dimen/activity_vertical_margin"
        android:paddingBottom="@dimen/activity_vertical_margin"
        tools:context="cn.tommyyang.buttondemo.MainActivity">

    <TextView
            android:id="@+id/text"
            android:text="Button点击事件以及响应的四种方式"
            android:textSize="24sp"
            android:gravity="center_horizontal"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"/>

    <TextView
            android:id="@+id/display"
            android:textSize="16sp"
            android:gravity="center_horizontal"
            android:layout_below="@id/text"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"/>

    <Button
            android:id="@+id/button1"
            android:text="方式一"
            android:textSize="18sp"
            android:layout_below="@id/display"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"/>

    <Button
            android:id="@+id/button2"
            android:text="方式二"
            android:textSize="18sp"
            android:layout_below="@id/button1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"/>

    <Button
            android:id="@+id/button3"
            android:text="方式三"
            android:textSize="18sp"
            android:layout_below="@id/button2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"/>

    <Button
            android:id="@+id/button4"
            android:text="方式四"
            android:textSize="18sp"
            android:layout_below="@id/button3"
            android:onClick="button4"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"/>
\</RelativeLayout>


<p>源码下载地址: <a href="https://github.com/joyang1/ButtonClickDemo/" target="_blank" title="ButtonDemo">查看</a> </p>
