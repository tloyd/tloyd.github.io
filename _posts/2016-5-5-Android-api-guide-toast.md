---
layout: post
title:  "Android API Guide:Toast 中文翻译"
date:   2016-05-05 21:33:53
categories: 文档翻译
tags:  AndroidAPI Toast
author: 何帆
---

* content
{:toc}  

Toast为应用操作提供了一种简单的反馈机制，它是一个小型的弹出窗口……





> A toast provides simple feedback about an operation in a small popup. It only fills the amount of space required for the message and the current activity remains visible and interactive. For example, navigating away from an email before you send it triggers a "Draft saved" toast to let you know that you can continue editing later. Toasts automatically disappear after a timeout.

Toast为应用操作提供了一种简单的反馈机制，它是一个小型的弹出窗口，只占用足够其显示消息内容的空间，而不影响当前activity的可见与交互。例如，它会在你退出未发送的邮件时，触发一条内容为"draft saved"的toast提示你能稍后继续编辑这条邮件。toast会自动在超时后消失。 

![](http://ww4.sinaimg.cn/mw690/005JzrjDjw1f3m0g1vxs9j309b06aglt.jpg) 
 
> If user response to a status message is required, consider instead using a Notification.

如果需要用户回馈一个状态消息，可以考虑使用[notification](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)。

## The Basics基本用法
--- 
> First, instantiate a Toast object with one of the makeText() methods. This method takes three parameters: the application Context, the text message, and the duration for the toast. It returns a properly initialized Toast object. You can display the toast notification with show(), as shown in the following example:

首先，你需要用`maketext()`方法来实例化一个toast对象，这个方法有三个参数:应用的context，需要显示的文本消息，和需要显示的时间。它的返回值是一个实例化的toast对象。用`show()`方法来显示。如下所示: 

    Context context = getApplicationContext();
    CharSequence text = "Hello toast!";
    int duration = Toast.LENGTH_SHORT;
    
    Toast toast = Toast.makeText(context, text, duration);
    toast.show();

> This example demonstrates everything you need for most toast notifications. You should rarely need anything else. You may, however, want to position the toast differently or even use your own layout instead of a simple text message. The following sections describe how you can do these things.
> 
> You can also chain your methods and avoid holding on to the Toast object, like this:
  
这个示例展示了绝大多数toast所需要的一切，你几乎不需要其他的了。然后，你或许想指定toast需要显示的位置，或者自定义toast的布局样式，接下来的部分将描述如何使用它们。  

你也可以链接这些方法来避免在内存里保留一个toast对象：

    Toast.makeText(context, text, duration).show();

## Positioning your Toast 指定toast的位置
--- 
> 
> A standard toast notification appears near the bottom of the screen, centered horizontally. You can change this position with the setGravity(int, int, int) method. This accepts three parameters: a Gravity constant, an x-position offset, and a y-position offset.
> 
> For example, if you decide that the toast should appear in the top-left corner, you can set the gravity like this:

一个标准的toast出现在靠近屏幕下方垂直居中的位置。你可以使用`setGravity(int, int, int)`方法改变他的位置，这个方法有3个参数。一个gravity常量，一个x轴偏移量，和一个y轴偏移量。

例如，如果你想让一个toast出现在屏幕的右上角。你可以这么设置参数：

    toast.setGravity(Gravity.TOP|Gravity.LEFT, 0, 0);

> If you want to nudge the position to the right, increase the value of the second parameter. To nudge it down, increase the value of the last parameter.

如果你想向右移动位置，增加第二个参数的值；向下移动位置，增加第三个参数的值

## Creating a Custom Toast View 创建一个自定义的Toast视图
--- 
> If a simple text message isn't enough, you can create a customized layout for your toast notification. To create a custom layout, define a View layout, in XML or in your application code, and pass the root View object to the setView(View) method.
> 
> For example, you can create the layout for the toast visible in the screenshot to the right with the following XML (saved as toast_layout.xml):

如果一个简单的文本消息无法满足需求。你可以为你的toast指定一个布局。创建一个自定义布局，你需要定义一个xml布局文件。并且调用`setView(View)`方法。将根节点view当做参数传递给它。

例如，你可以用下面的代码为toast创建一个布局文件（保存为toast_layout.xml）：

    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:id="@+id/toast_layout_root"
      android:orientation="horizontal"
      android:layout_width="fill_parent"
      android:layout_height="fill_parent"
      android:padding="8dp"
      android:background="#DAAA"
      >
    <ImageView android:src="@drawable/droid"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:layout_marginRight="8dp"
       />
    <TextView android:id="@+id/text"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:textColor="#FFF"
      />
    </LinearLayout>

> Notice that the ID of the LinearLayout element is "toast_layout_root". You must use this ID to inflate the layout from the XML, as shown here:

注意这个LinearLayout元素的ID是“toast\_layout\_root”.你需要使用这个ID去膨胀这个布局，如下所示：

    LayoutInflater inflater = getLayoutInflater();
    View layout = inflater.inflate(R.layout.custom_toast,
       (ViewGroup) findViewById(R.id.toast_layout_root));
    
    TextView text = (TextView) layout.findViewById(R.id.text);
    text.setText("This is a custom toast");
    
    Toast toast = new Toast(getApplicationContext());
    toast.setGravity(Gravity.CENTER_VERTICAL, 0, 0);
    toast.setDuration(Toast.LENGTH_LONG);
    toast.setView(layout);
    toast.show();

> First, retrieve the LayoutInflater with getLayoutInflater() (or getSystemService()), and then inflate the layout from XML using inflate(int, ViewGroup). The first parameter is the layout resource ID and the second is the root View. You can use this inflated layout to find more View objects in the layout, so now capture and define the content for the ImageView and TextView elements. Finally, create a new Toast with Toast(Context) and set some properties of the toast, such as the gravity and duration. Then call setView(View) and pass it the inflated layout. You can now display the toast with your custom layout by calling show().

首先，调用`getLayoutInflater()`或者`getSystemService()`方法获得LayoutInflater对象，然后用`inflate(int, ViewGroup)`方法膨胀这个布局，第一个参数是需要膨胀的的XML布局ID，第二个参数是父视图。你可以得到这个膨胀过的layout里的其他视图对象。现在，得到并设置`ImageView`和`TextView`的内容。最后，用构造方法`Toast(Context)`创建一个toast对象，并且设置一些所需的属性例如位置，时长，然后调用`setView(View)`方法并将膨胀后的layout当做参数传递给它。现在你可以调用`show()`方法来显示你自定义的toast了

> Note: Do not use the public constructor for a Toast unless you are going to define the layout with setView(View). If you do not have a custom layout to use, you must use makeText(Context, int, int) to create the Toast.

注意：除非你打算用`setView(View)`方法来来定义Toast的布局，否则请勿使用toast的公共构造方法创建一个Toast。如果你没有自定义Toast的布局，请务必使用`makeText(Context, int, int)`方法来创建一个Toast。