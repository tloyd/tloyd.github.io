---
layout: post
title:  "Servlet的一些知识点之ServletConfig对象"
date:   2016-9-12 21:44:13
categories: 经验分享
tags:  JavaEE Servlet JavaWeb
---

* content
{:toc}

> 在Servlet的配置文件中，可以用一个或多个<init-param>标签为Servlet配置一些初始化参数，在web容器创建Servlet实例时，会根据这个初始化参数自动将这些初始化参数封装到ServletConfig对象中去，并在调用Servlet的init方法时，将ServletConfig对象传递给Servlet。进而，程序猿就可以通过ServletConfig对象得到Servlet的初始化参数呵呵~




- 这个<init-param>标签是配置在这个Servlet标签里的一个标签，这个标签包括一个param-name标签跟一个param-value标签
- 在init()方法里得到ServletConfig对象
- 用` getInitParameter(String ParameterName)`方法得到参数
- 在实际开发中直接HttpServlet.getServletConfig()就可以得到ServletConfig对象了（父类封装好的
- `java.util.Enumeration<java.lang.String> getInitParameterNames()`得到所有的参数名称





