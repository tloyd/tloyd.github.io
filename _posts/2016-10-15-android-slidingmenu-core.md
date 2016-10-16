---
layout: post
title:  "自定义SlidingMenu的一些知识点"
date:   2016-10-15 21:06:45
categories: 经验分享
tags: Android 安卓
author: 何帆
---

* content
{:toc}

> 很多APP都有侧滑菜单的功能，部分APP左右都是侧滑菜单,很多开源项目可以很好帮助我们实现侧滑功能.我试图自己写了一个SlidingMenu, 为了避免被人诟病重复造轮子,我只实现了其中的一些核心功能,这里做个知识笔记





## 如何自定义ViewGroup的子类
- 如何在一个ViewGroup里实现子View的显示
    1. 首先得获得子View的引用,一般是在`onFinishInflate()`里初始化子View,该方法是在xml标签执行结束后执行,此时会知道自己有几个子View,可以使用`getChildAt(int index)`方法来获取子View的引用
    2. 然后override`onMeasure(int widthMeasureSpec, int heightMeasureSpec)`方法, 在该方法里去测量子View.基础做法是自己去构建高与宽的measureSpec对象,然后调用`view.measure(int widthMeasureSpec, int heightMeasureSpec)`方法来测量子View
    在这里,如果不需要特别复杂的测量规则,可以直接调用`ViewGroup.measureChild(View child, int parentWidthMeasureSpec,int parentHeightMeasureSpec)`来测量子View.
- 在自定义ViewGroup的时候，如果对子View的测量没有特殊的需求，那么可以继承系统已有的布局(比如FrameLayout)，目的是为了让已有的布局帮我们实行onMeasure;


--- 

## ViewDragHelper类的简单使用


> ViewDragHelper是一个用于编写自定义ViewGroups的实用程序类。它提供了许多有用的操作和状态跟踪，以允许用户在其父ViewGroup中拖动和重新定位ViewGroup中的子视图

- 使用步骤:
    1. 创建实例
    2. 在`ViewGroup.onInterceptTouchEvent(MotionEvent event)`和`ViewGroup.onTouchEvent(MotionEvent event)`中拦截ViewGroup的触摸事件并传递给ViewDragHelper处理
    3. 创建ViewDragHelper.CallBack实例并复写其中的相关方法


    	重点介绍几个方法
        
	    // 当捕获的视图的位置由于拖动或平移而改变时调用
	    public void onViewPositionChanged(View changedView, int left, int top, int dx, int dy) {}

		// 当子View不再活动即被放开时候调用该方法,也支持甩动操作
		public void onViewReleased(View releasedChild, float xvel, float yvel) {}

		// 得到一个可以拖拽的View的可移动范围,但是这两个方法不会对移动范围起限制
		public int getViewHorizontalDragRange(View child) {
		    return 0;
		}

		// 复写该方法提供横向拖放的数值
		public int clampViewPositionHorizontal(View child, int left, int dx) {
		    return 0;
		}

--- 

## 补充知识点

- 在使用AppCompactActivity的时候,如果需要去除掉titlebar,在style里设置`<item name="android:windowNoTitle">true</item>`往往是无效的,这个属性只对Activity起作用,如果需要使用AppCompactActivity去除标题,要设置`<item name="windowNoTitle">true</item>`,windowNoTitle这个属性是在appcompat-v7中的属性在appcompat-v7\res\values\values.xml中定义的;
- 在使用系统自定义的BaseAdapter的实现类时,如果不满意其中的显示效果,可以直接重写`getView(int position, View convertView, ViewGroup parent)`方法,自定义样式
- 设置一张Drawable的遮罩层的方法`Drawable.setColorFilter (int color, PorterDuff.Mode mode)`