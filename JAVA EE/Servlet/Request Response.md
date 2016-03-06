##Request 和 Response##
#**Servlet配置问题**

>1，关于默认的servlet如果某个Servlet的映射路径它是 "/" 那么它就是默认Servlet.默认的Servlet是用于处理不能处理的请求。			

>2，在tomcat中conf/web.xml文件，我们自己工程的所有的web.xml相当于继承了这个xml.

>3,当我们去访问web工程下内容时，都是通过web.xml文件中的<url-pattern>去查找到资源。而我们在访问  a.txt  a.html时在xml文件中没有与其对应的资源，这时，是默认的servlet进行处理。

--------------------
#Response和Request体系
--------------------
##**1,什么是Request和Response**

>Response:相应对象:httpServletResponse

>Request:请求对象: httpServletRequest

##**2,Response和Request分别代表什么?什么时候创建?**

>Web服务器收到客户端的http请求，会针对每一次请求，分别创建一个用于代表请求的request对象、和代表响应的response对象

>对于请求与响应对象，是每一次请求都会创建新的对象。当响应结束，请求与响应对象自动消失。它们是由服务器创建的。
		
##request与response的作用：
>request可以处理所有的http请求.  简单说:request可以获取我们http请求信息。response可以处理所有的http响应。简单说:response可设置http响应信息。
    

#**一，Request**
--------------------


#**二，Response**

--------------------

>1.response对象，它是在service方法中接收到的。
		
>resposne对象它是用于设置http响应的。
		
>2.response操作响应信息.
  >1.响应状态行的状态码   setStatus
  >2.响应头  setHeader  setDateHeader  
  			
		//设置状态码
		response.setStatus(302);
		//设置响应头
		response.setHeader("location","www.baidu.com");
			response.sendRedirect("url");//域名重定向
>setIntHeader

  >3.响应正文  是通过流操作   
getOutputStream()   getWriter();			

  >4.示例1
通过response实现重定向		
服务器端要做两件事:
1.状态码  302
2.响应头   location
重定时，可以访问站内资源，也可以访问站外资源。>开发中要想重定向   respons.sendRedirect(String location);
>5.示例 2发送http头，控制浏览器定时跳转网页
response.setHeader("refresh", "5;url=/day8_1/refresh.html");
通知浏览器5秒后跳转到url指定的路径。
		
>扩展:在实际操作中，很少使用服务器端进行定时跳转，一般都是在浏览端完成。
可以通过js完成数字变化
    		<script type="text/javascript">
    			var time=5;
    			function change(){
    				var span=document.getElementById("s");
    				span.innerHTML=time;
    				time--;
    				setTimeout("change()", 1000);
    			}
    		</script>
    		<meta http-equiv="refresh" 
    
    content="5;url=/day8_1/index.jsp">
    		
    		<body onload="change();">
    			页面会在<span id="s"></span>后跳转到
    
    index.jsp。
    		</body>

