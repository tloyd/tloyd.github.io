---
layout: post
title:  "AndroidStudio无法用logcat输入xml的错误"
date:   2015-06-14 23:00:27
categories:  奇葩Bug集锦
tags: logcat JavaScript AndroidStudio logcat XML
---

* content
{:toc}

## AndroidStudio无法用logcat输入xml的错误
###### 问题表现:
- 在写通过网络用AsyncTask加载解析XML数据时候,在服务器端用字符串的方式讲XML数据写回到客户端,保险起见,想先把XML数据通过log打印出来确定下能够加载,却死活无法输出.而直接通过浏览器的方式又可以正常取回servlet写回的XML文件数据,用各种关键字也没有找到有人有类似的问题,




###### 问题分析:
- 后来直接断点调试,发现其实XML返回的字符串其实是已经取到了,只是无法用log输出.我又用subString截取字符串的方式,发现长度超过38?个字节的字符串就无法打印了.问题是如果打印普通的字符,超过38个字节也都能正常输出到logcat上.不过无论如何,取回的数据还是可以解析使用的.如果有人知道具体原因还烦请告知讨论下
