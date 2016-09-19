---
layout: post
title:  "Servlet的一些知识点之ServletContext对象"
date:   2016-9-18 21:58:22
categories: 经验分享
tags: JavaEE Servlet JavaWeb
author: 何帆
---

* content
{:toc}

>servletContext接口是Servlet中最大的一个接口，呈现了web应用的Servlet视图。ServletContext实例是通过 getServletContext()方法获得的，由于HttpServlet继承GenericServlet的关系，GenericServlet类和HttpServlet类同时具有该方法





- 当服务器调用一个Servlet的时候，会传递给他四个对象
Request Response ServletConfig ServletContext
- 这是一个容器
- Web服务器启动时，他会为每个Web应用创建一个对应的ServletContext对象 代表的是当前的web应用
- 开发人员可以调用`ServletConfig.getServletContext()`这个方法得到ServletContext对象
- 一个web应用共享一个Servletcontext对象， 所有的Servlet通过ServletContext实现数据共享，这个ServletContext被称为Context域对象（request page session）
- 在web.xml里可以配置整个web应用的初始化参数，用\<context-param\>标签，用ServletContext的`getInitParameter(String param-name)`来获取
- 重定向是两次请求，客户机自己去二次访问服务器，转发是一次请求，服务器自己去得到其他资源放回给客户机
- 我们在Java开发中基本上配置文件用的都是.xml跟.properties文件,可以用ServletContext来读取资源文件
- web应用中调用普通java程序应该用类装载器来读取资源文件,不能把Context传过去,这种写法格式不良好,`class.getClassLoader().getResourceAsStream(String path)`这个方法,但是这个文件不能太大,而且这个方法获取的资源文件是静态的,在这个程序运行中, 就算你改变了资源文件, 也不会读取到更新后的资源文件.
- 如果需要读取到实时的资源文件, 应该用`class.getClassLoader().getResource(String path).getPath()`来得到一个URL,然后用普通的FileInputStream来读取这个文件



