---
layout: post
title:  "Servlet的一些知识点之Response与Request对象"
date:   2016-10-2 17:49:42
categories: 经验分享
tags:  JavaEE Servlet JavaWeb
---

* content
{:toc}

>web服务器收到客户端的HTTP请求，会针对每一次请求分别创建一个用于代表请求的request对象和代表响应的response对象。
1. 要得到客户机提交过来的数据，只需要找request对象就行了。
2. 要向客户机输出数据，只需要找response对象就行了。




### Response对象:
___
- 设置Response使用的码表,控制Response以什么码表向浏览器输出数据
   `response.setCharacterEncoding("utf-8");`
- 指定浏览器使用什么码表打开服务器所发送的数据
    `response.setHeading("content-type","text/html;charset=utf-8"); // 这里是分号`
    或者
    `response.setContentType("text/html;charset=utf-8");`
- 你还可以使用`<meta>`标签来设置一个html头来控制浏览器使用什么码表打开服务器发送的数据
    `<meta http-equiv='content-type' content='text/html;charset=utf-8'>`
- 设置头来实现下载文件的功能
    `response.setHeader("content-disposition","attachment;filename="+filename);`
    然后在用ServletContext对象与io流去读取文件并写入到response中去
- 如果下载的文件名是中文, 则需要加入对中文文件名的处理
     `response.setHeader("content-disposition","attachment;filename="+URLEncoder.encode(filename,"UTF-8"));`
- 在Servlet里无法通过设置heading的方式来设置刷新跳转到JSP,要通过设置`<meta>`方式来控制跳转到JSP页面
    `<meta http-equiv='refresh' content='3;url=/demo/index.jsp'>`
- 用expires控制浏览器缓存
- 设置请求重定向(能不用尽量不用)
   - 一种可以用设置头的方式
    `response.setStatus(302);
    response.setHeader("location","/demo/index.jsp");`
    - 另一种用response自己的API
    `response.setRedirect("/demo/index.jsp");`

### Request对象:
___
- request也是一个域
- request实现请求转发和带数据转发
    `request.setAttribute("data","data");
request.getRequestDispatcher("/message.jsp").forward(request,response);`
    然后在JSP里用**EL表达式**取出并显示数据
- request请求转发的细节:
    - forward方法用于将请求转发到RequestDispatcher对象封装的资源
    - 如果在调用forward方法之前, 在Servlet程序中写入的部分内容已经被真正的传送到了客户端,forward方法将抛出IllegalStateException异常
    - 如果在调用forward方法前向Servlet缓冲区写入数据,但是还没被刷新到客户端,forward方法就会正常执行,原来写入缓冲区的数据会被清空掉, 但是写入HttpServletResponse对象中的响应头字段将会被保留
- request转发的特点(区别于重定向):
    - 客户机只有一次请求,但是服务器有多次资源调用
    - 地址栏没有发生变化
- RequestDispatcher中的include(request,response)方法:
    `request.getRequestDispatcher("/public/head.jsp").include(request,response); // 此方法用来包含一些不变的页面`
- 解决Request的乱码问题:
    - 通过post方式提交数据给Servlet
    `public void doPost(httpServletRequest request, httpServletResponse response) throws ServletException, IOException{
             //在获取用户表单信息之前把request的码表设置成UTF-8，
             //如果没这句的话，如果提交中文信息的时候，会照成乱码。
             request.setCharacterEncoding("UTF-8");            
             String value = request.getParameter("username"); //从request中获取客户端提交过来的信息
             System.out.println(value);
       }`
   - 通过get方式提交数据给Servlet (要手动处理)
   `public void doGet(httpServletRequest request, httpServletResponse response) throws ServletException, IOException{
             //从request中获取客户端提交过来的中文信息，获取到乱码 
             String value = request.getParameter("username");
             //拿到乱码反向查找 iso-8859-1 码表，获取原始数据，
             //在构造一个字符串让它去查找UTF-8 码表，已得到正常数据
             value1 = new String (value.getBytes("iso-8859-1"), "UTF-8") ;
             System.out.println(value);         
                   }`
    - 用超链接提交表单信息(通过 get 方式提交，需调用到doGet方法)
- web工程中各种地址的写法:
    **原则:**就看给谁用的,先打一个/,如果给服务器用的,/.就代表当前应用,如果是给客户机用的,那/代表的是网站
   1. 服务器用的
   `request.getRequestDispatcher("/index.jsp").forward(request,response);`
   2. 客户机用的
   `response.sendRedirect("/demo/index.jsp")`
   3. 服务器用的
   `this.getServletContext().getRealPath("/index.jsp");`
    4. 服务器用的
    `this.getServletContext().getResourceAsStream("/index.jsp")`
    5. 客户机用的
    `<a href="/demo/index.jsp">点点</a>`
    6. 客户机用的
    `<form action="/demo/index.jsp"></form>`
- 防盗链
    `String referer = request.getHeader("referer"); 
    if(referer==null || !referer.startsWith("http://localhost:8080")){ 
        response.sendRedirect("http://localhost:8080/Request/index.jsp"); 
        return; 
    } 
    response.setCharacterEncoding("utf-8"); 
    response.setContentType("text/html;charset =utf-8"); 
    PrintWriter pw = response.getWriter(); 
        pw.write("喜剧片《东成西就》"); `






