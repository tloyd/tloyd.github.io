---
layout: post
title:  "Genymotion使用遇到问题与解决方法"
date:   2016-05-18 23:25:27
categories: 经验分享 错误集锦
tags:  Genymotion
author: 何帆
---

* content
{:toc}

记录一下使用Genymotion模拟器遇到的问题与解决方法,Genymotion确实是一个非常好的模拟器,快,轻巧,就是错误多,错误提示差,而且几乎无法得到官方的支持.



Genymotion的运行与错误信息日志记录在`C:\Users\administrator\AppData\Local\Genymobile\genymotion.log`里,不过一打开这个日志能直接晕过去,信息堆一起根本无法辨识,因此还是要把遇到的问题一样样记录下来,以后作为一个参考  

**问题1**:Windows7下直接下载官网与VirtualBox打包的版本,出现死活无法打开VirtualBox的情况,VirtualBox的版本应该是5.0.1,我后来换了Genymotion的版本,也换了VirtualBox的版本,确定是VirtualBox的这个5.0.1的有问题,我后来是换回了稳定的4.2.12

**问题2**:这个问题更奇葩,Windows10的环境下,有时候无法加载虚拟系统,提示无法打开VirtualBox,然后多启动个一两次又可以了,刚刚想重启下截个图,又正常了,以后这些问题都要及时截图,太坑了  
![](http://ww3.sinaimg.cn/mw690/005JzrjDgw1f41pvs4j7gj30dy0ajaby.jpg)  
更新:截图就长这样了,也没什么可以参考的提示,我的做法是把log文件给删除了,就可以打开了

**问题3**:还有一个要注意的地方就是Genymotion对VirtualBox的网络配置是有要求的,如果是自己下的VirtualBox,最好要确认下网络配置,IP地址必须配成192.168.56.1;如下图所示的配置就可以了  
![](http://ww3.sinaimg.cn/mw690/005JzrjDgw1f414pdduy2j30dq07wt9k.jpg)  
![](http://ww2.sinaimg.cn/mw690/005JzrjDgw1f414pecdpqj30dq07w0ts.jpg)