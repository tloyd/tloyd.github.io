---
layout: post
title:  "在用jekyll搭建本地环境时候的一些问题"
date:   2016-05-01 00:03:01
categories:  奇葩Bug集锦
tags: ruby jekyll
---

* content
{:toc}

- 在用到`gem install bundler`的时候,一直产生fetch url error错误.网上有人说是因为gfw的原因,很多人建议用国内淘宝的镜像,然而我用却也是一直出现Error fetching的错误



- 如下:

> Error fetching https://ruby.taobao.org/:
> SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (https://rubygems-china.oss-cn-hangzhou.aliyuncs.com/specs.4.8.gz)

- 这个错误,taobao在github上的说法是OSS 的 SSL 证书供应商变动而出现的原因,建议把rubygems更新到2.6.3,我更新后还是不行.后来我换了中山大学的镜像[http://mirror.sysu.edu.cn/rubygems/](http://mirror.sysu.edu.cn/rubygems/ "http://mirror.sysu.edu.cn/rubygems/")才可以用的.

- 还有一点就是官方现在推荐的安装方式,是用github对应的一个gem `$ bundle update` 来打包一键设置环境,这个里面的有些依赖的包是不支持最新的`ruby2.3.1`的,所以建议大家还是用之前的版本就可以了,最好是2.2.4的版本,这个版本也自带了gem,也比较方便

- 另外一个问题是发表文章时候我遇到的,就是博文发表的时间是按照头信息 YAML里的date这个变量获取的,会覆盖掉文件名的时间.另外一点就是可能是github pages的时区问题,在date这个变量里不能设置当前本地的时间,如果直接设置当前本地的时间会导致文章无法显式,我猜测是因为时区的问题,因为美国的时区基本上比中国晚12小时,如果直接设置本地时间,github pages会认为这篇博文还不到发表时间,所以也就看不到了