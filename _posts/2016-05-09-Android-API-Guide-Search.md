---
layout: post
title:  "Android API Guide:Search 中文翻译"
date:   2016-05-09 10:06:43
categories: 文档翻译
tags:  AndroidAPI Search
author: 何帆
---

* content
{:toc}  

搜索，是安卓的一个核心功能。用户可以搜索他们所有能获得的数据，无论这些内容在设备上或者在网络上。




## Search Overview搜索概述  

> Search is a core user feature on Android. Users should be able to search any data that is available to them, whether the content is located on the device or the Internet. To help create a consistent search experience for users, Android provides a search framework that helps you implement search for your application.

搜索，是安卓的一个核心功能。用户可以搜索他们所有能获得的数据，无论这些内容在设备上或者在网络上。为了帮助开发者创建一个对用户来说一致的搜索体验，安卓提供了一个搜索框架，来帮助你为你的应用实现搜索功能。

> The search framework offers two modes of search input: a search dialog at the top of the screen or a search widget (SearchView) that you can embed in your activity layout. In either case, the Android system will assist your search implementation by delivering search queries to a specific activity that performs searchs. You can also enable either the search dialog or widget to provide search suggestions as the user types. Figure 1 shows an example of the search dialog with optional search suggestions.

搜索框架提供了两个输入模式，一个是在屏幕顶端的搜索框，另外一个是搜索插件，你能把它嵌入到你的活动布局中。任何情况下，安卓系统会通过为您将查询信息传递给指定的活动执行搜索来为你实现搜索功能。你也可以为搜索框或搜索插件开启搜索建议功能。下图显示一个使用搜索建议的例子。

![图1](http://ww2.sinaimg.cn/mw690/005JzrjDjw1f3pgf490npj306y0blt9e.jpg)

> Once you've set up either the search dialog or the search widget, you can:
> 
> * Enable voice search
> * Provide search suggestions based on recent user queries
> * Provide custom search suggestions that match actual results in your application data
> * Offer your application's search suggestions in the system-wide Quick Search Box

一旦你设置了搜索框或搜索插件，你就可以：  

* 开启语音搜索  
* 根据用户最近的查询提供搜索建议  
* 根据应用数据来提供自定义的搜索建议来匹配真实的结果  
* 向系统搜索栏提供你的应用搜索建议  

> **Note**: The search framework does not provide APIs to search your data. To perform a search, you need to use APIs appropriate for your data. For example, if your data is stored in an SQLite database, you should use the [android.database.sqlite](https://developer.android.com/reference/android/database/sqlite/package-summary.html) APIs to perform searches.  
>  
> Also, there is no guarantee that a device provides a dedicated SEARCH button that invokes the search interface in your application. When using the search dialog or a custom interface, you must provide a search button in your UI that activates the search interface. For more information, see [Invoking the search dialog](https://developer.android.com/intl/zh-cn/guide/topics/search/search-dialog.html#InvokingTheSearchDialog).

注意，这个搜索框架不提供用来搜索应用数据的api. 为了执行搜索操作，你需要使用合适的api来搜索你的数据。比如，如果你的数据保存在SQLite数据库中，你需要使用[android.database.sqlite](https://developer.android.com/reference/android/database/sqlite/package-summary.html)的api在执行搜索操作。  

而且，我们不保证设备上会提供一个，单独的搜索按钮来为你的应用调用搜索功能。当你需要使用搜索框，或者自定义的搜索界面时，你需要在你的应用界面上提供一个搜索按钮来呼出搜索界面，更多信息，请查看[调用搜索框](https://developer.android.com/intl/zh-cn/guide/topics/search/search-dialog.html#InvokingTheSearchDialog)。

> The following documents show you how to use Android's framework to implement search:
> 
> [Creating a Search Interface](https://developer.android.com/intl/zh-cn/guide/topics/search/search-dialog.html)  
> 　　How to set up your application to use the search dialog or search widget.  
> [Adding Recent Query Suggestions](https://developer.android.com/intl/zh-cn/guide/topics/search/adding-recent-query-suggestions.html)  
> 　　How to provide suggestions based on queries previously used.  
> [Adding Custom Suggestions](https://developer.android.com/intl/zh-cn/guide/topics/search/adding-custom-suggestions.html)  
> 　　How to provide suggestions based on custom data from your application and also offer them in the system-wide Quick Search Box.  
> [Searchable Configuration](https://developer.android.com/intl/zh-cn/guide/topics/search/searchable-config.html)  
> 　　A reference document for the searchable configuration file (though the other documents also discuss the configuration file in terms of specific behaviors).

接下来的文档将为你演示，如何使用安卓的搜索框架来实现搜索功能：

[创建一个搜索界面](https://developer.android.com/intl/zh-cn/guide/topics/search/search-dialog.html)  
　　如何设置你的应用程序去使用搜索框或者搜索插件  
[添加最近搜索建议 ](https://developer.android.com/intl/zh-cn/guide/topics/search/adding-recent-query-suggestions.html)  
　　如何基于之前的搜索记录来提供搜索建议  
[增加自定义的搜索建议](https://developer.android.com/intl/zh-cn/guide/topics/search/adding-custom-suggestions.html)  
　　如何基于当前应用数据来提供搜索建议，并把搜索建议提供给系统搜索框  
[搜索配置](https://developer.android.com/intl/zh-cn/guide/topics/search/searchable-config.html)  
　　一个搜索配置的参考文档(尽管其他的文档针对某些特定的行为也探讨了如何配置搜索文件)

### Protecting User Privacy保护用户隐私
---

> When you implement search in your application, take steps to protect the user's privacy. Many users consider their activities on the phone—including searches—to be private information. To protect each user's privacy, you should abide by the following principles:

当你在你的应用程序中使用搜索功能，你需要采取以下几步来保护用户的隐私。很多用户认为他们在手机上的行为包括搜索是一种隐私信息。为了保护每个用户的隐私，你需要遵从以下几点原则：  

> * **Don't send personal information to servers, but if you must, do not log it.**  
> Personal information is any information that can personally identify your users, such as their names, email addresses, billing information, or other data that can be reasonably linked to such information. If your application implements search with the assistance of a server, avoid sending personal information along with the search queries. For example, if you are searching for businesses near a zip code, you don't need to send the user ID as well; send only the zip code to the server. If you must send the personal information, you should not log it. If you must log it, protect that data very carefully and erase it as soon as possible.

* **不要将个人信息发送给服务器，如果您一定要这么做，千万不要记录下来**  
个人信息是指，任何能够识别用户的信息。比如名字电子邮箱。账单信息。或者其他能够合理关联到这些信息的数据。如果你的搜索实现需要服务器的支持，避免将搜索数据与个人信息一起发送给服务器。例如。假设你在根据邮政编码搜索附近的企业，你不需要发送用户的id. 只需要发送邮政编码给服务器，而且请勿记录下来。如果你必须记录，请非常小心的保护它并尽可能快的删除掉这个信息。

> * **Provide users with a way to clear their search history.**
> The search framework helps your application provide context-specific suggestions while the user types. Sometimes these suggestions are based on previous searches or other actions taken by the user in an earlier session. A user might not wish for previous searches to be revealed to other device users, for instance, if the user shares the device with a friend. If your application provides suggestions that can reveal previous search activities, you should implement the ability for the user to clear the search history. If you are using [SearchRecentSuggestions](https://developer.android.com/reference/android/provider/SearchRecentSuggestions.html), you can simply call the `clearHistory()` method. If you are implementing custom suggestions, you'll need to provide a similar "clear history" method in your content provider that the user can execute.

* **为用户提供一个清除搜索历史的方式**  
谷歌的搜索框架帮助你的应用提供根据上下文的搜索建议。有时这些搜索建议是基于之前的搜索记录，或者用户在先前其他会话的操作。一些用户或许不愿意将搜索记录透露给其他的设备用户，例如，有时用户将当前设备分享给一个朋友。如果你的应用程序会泄漏历史搜索活动记录，你需要为用户提供一个清除历史的方法，如果你调用的是[SearchRecentSuggestions](https://developer.android.com/reference/android/provider/SearchRecentSuggestions.html)类，你可以简单的调用`clearHistory()`方法来实现这个功能。如果你提供的是自定义的搜索建议，你也需要在你的内容提供者中提供一个用户可以执行的类似方法来清除历史纪录。
