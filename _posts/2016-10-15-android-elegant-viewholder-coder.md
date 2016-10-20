---
layout: post
title:  "更简洁优雅的ViewHolder写法"
date:   2016-10-20 00:25:01
categories: 经验分享
tags: Android 安卓
author: 何帆
---

* content
{:toc}

> 在我们自定义Adapter的时候,总免不了要写ViewHolder,ViewHolder写的多了,难免觉得麻烦,当然如果用一些插件自动生成Holder,也可以减少一些工作量.但是我们手工写的话,还是要进行一些性能优化,减少不必要的重复操作,精简代码.




废话不多说,直接看代码

    static class ViewHolder {
        TextView tvName, tvFirstLetter;

        public ViewHolder(View convertView) {
            tvName = (TextView) convertView.findViewById(R.id.tv_name);
            tvFirstLetter = (TextView) convertView.findViewById(R.id.tv_first_letter);
        }

        public static ViewHolder getHolder(View convertView) {
            ViewHolder holder = (ViewHolder) convertView.getTag();
            if (holder == null) {
                holder = new ViewHolder(convertView);
                convertView.setTag(holder);
            }
            return holder;
        }
    }

在`getView(int position, View convertView, ViewGroup parent)`里是这么调用的

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        if (convertView == null) {
            convertView = View.inflate(context, R.layout.adapter_listview, null);
        }
        ViewHolder holder = ViewHolder.getHolder(convertView);
        // 这里可以进行holder的数据填充
		return convertView;
    }

这么写的好处就是将繁琐的findViewById操作直接都放到ViewHolder里了,而且是一次性的,让getView方法显得无比的简洁
