---
layout: post
title:  "Servlet的一些知识点"
date:   2016-9-11 20:44:13
categories: 经验分享
tags:  JavaEE Servlet JavaWeb
---

* content
{:toc}

> 在学习JavaWeb过程中一些觉得需要记下来的知识点呵呵~




## web.xml的配置
- servlet标签中配置的是servlet这个类的名称 以及对外的命名
- servlet-mapping标签中配置的是如何映射到servlet标签中配置的servlet

    ![图1](http://ww3.sinaimg.cn/mw690/005JzrjDgw1f7s8zjp316j30d504kjsd.jpg)

-  并且可以配置多个servlet-mapping标签，其中url-pattern可以使用通配符，但是只能使用/*或者*.?的形式
- 通常一个servlet都是在被客户机访问的时候才初始化创建的，而在servlet里配置<load-on-start>标签，可以让这个servlet在服务器启动的时候被创建，这样可以让servlet相应更快，避免servlet在被客户端访问的时候才初始化。
- 映射路径为一个/正斜杠，那么这个servlet就是当前web应用的缺省servlet，它用来处理所有servlet都无法相应的servlet，可以用来处理404页面
- 在没有配置缺省servlet的时候，服务器会配置一个默认的缺省servlet，这个缺省的servlet负责为客户机寻找web应用内静态的web资源
- 在tomcat目录里的conf文件夹下的配置，会被所有的web应用所共享，而默认的缺省servlet正是在这个文件夹下的web.xml文件里配置的，而且这个缺省servlet已经被配置成在服务器启动的时候就初始化了
## servlet中要注意的问题
- servlet线程安全问题，可以用synchronized来做同步代码块，但是通常不这么做也不能这么做。通常是让这个servlet继承一个标记接口SingleThreadModel。
- 子类在继承父类的方法，不能抛出比父类更多的异常（这个其实是Java基础的要点
- Servlet生命周期分为三个阶段：
    1. 初始化阶段  调用init()方法
    2. 响应客户请求阶段　　调用service()方法
    3. 终止阶段　　调用destroy()方法

