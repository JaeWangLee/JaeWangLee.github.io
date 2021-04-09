---
title: "[강의노트] Servlet 데이터 공유"
excerpt: "Servlet 데이터 공유에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - JSP
tags:
  - JSP
  - Servlet
  - 웹프로그램
last_modified_at: 2021-04-07 20:36:20
---
# 12. Servlet 데이터 공유

<span style="color:grey">[실전 JSP (renew ver.) - 신입 프로그래머를 위한 강좌] 내용입니다.</span>
  
## 12-1. servlet parameter
해당 서블릿 내에서만 공유하는 방법
![이미지](/assets/images/JSP&Servlet/실전JSP/12강/12강_1.png)
web.xml파일에 init parameter를 기술한 후,  
---
[web.xml]
```xml
  <servlet>
  	<servlet-name>servletEx</servlet-name> 
  	<servlet-class>com.servlet.ServletEx</servlet-class>
  	<init-param>
  		<param-name>adminId</param-name>
  		<param-value>admin</param-value>
  	</init-param>
  	<init-param>
  		<param-name>adminPw</param-name>
  		<param-value>1234</param-value>
  	</init-param>
  </servlet>
   <servlet-mapping>
  	<servlet-name>servletEx</servlet-name>
  	<url-pattern>/se</url-pattern>
  </servlet-mapping>

```
  
Servlet config 메소드를 이용하여 데이터를 불러옵니다. 
[ServletEx]
```java

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	//getServlet.Config().getInitParameter 를 이용!
		String adminId = getServletConfig().getInitParameter("adminId");
		String adminPw = getServletConfig().getInitParameter("adminPw");
		
		PrintWriter out = response.getWriter();
		out.print("<p>adminId : " + adminId + "</p>");
		out.print("<p>adminPw : " + adminPw + "</p>");
	}
```
서버를 돌리게 되면, 다음의 값을 확인할 수 있습니다. 
![이미지](/assets/images/JSP&Servlet/실전JSP/12강/12강_2.png)
  
> URL Mapping을 해놨으므로 /se로 접속하였음을 확인할 수 있다.  

## 12-2. context parameter
컨텍스트 전체적으로 데이터를 공유하는 방법 중   
컨테스트 파라메터를 지정해놓고 데이터를 공유하는 방법  
![이미지](/assets/images/JSP&Servlet/실전JSP/12강/12강_3.png)
web.xml파일에 context parameter를 기술한 후,  
[web.xml]  
```xml
  <context-param>
  	<param-name>imgDir</param-name>
  	<param-value>/upload/img</param-value>
  </context-param>
  
  <context-param>
  	<param-name>testServerIP</param-name>
  	<param-value>127.0.0.1</param-value>
  </context-param>
```
  
Context를 불러오기때문에 getServletContext 메소드를 이용하여 데이터를 불러옵니다.  
[ServletEx]  
```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		String imgDir = getServletContext().getInitParameter("imgDir");
		String testServerIP = getServletContext().getInitParameter("testServerIP");

		PrintWriter out = response.getWriter();		
		out.print("<p>imgDir : " + imgDir + "</p>");
		out.print("<p>adminPw : " + testServerIP + "</p>");
	}
```
결과는.. 다음과 같이 나옴을 확인할 수 있습니다.  
![이미지](/assets/images/JSP&Servlet/실전JSP/12강/12강_4.png)


## 12-3. context attribute
컨텍스트 전체적으로 데이터를 공유하는 방법 중  
set과 get으로 정보를 저장해놓고 공유해주는 방식!  
![이미지](/assets/images/JSP&Servlet/실전JSP/12강/12강_5.png)  
setAttribute를 사용하여 정보를 저장해놓습니다.  
  
[ServletEx]  
```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		getServletContext().setAttribute("connectedIP", "165.62.58.33");
		getServletContext().setAttribute("connectedUser", "gildong");
	}
```
다른 서블릿에서 getAttribute를 사용하여 정보를 가져옵니다.  
[ServletGet]
```java
@WebServlet("/seg")
public class ServletGet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		String connectedIP =(String)getServletContext().getAttribute("connectedIP");
		String connectedUser =(String)getServletContext().getAttribute("connectedUser");
		
		PrintWriter out =response.getWriter();
		out.print("<p>connectedIP : " + connectedIP + "</p>");
		out.print("<p>connectedUser : " + connectedUser + "</p>");
	}
```
![이미지](/assets/images/JSP&Servlet/실전JSP/12강/12강_6.png)  
  

### 참고. 전체 Code 

web.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>servletDataPjt</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  
  <context-param>
  	<param-name>imgDir</param-name>
  	<param-value>/upload/img</param-value>
  </context-param>
  
  <context-param>
  	<param-name>testServerIP</param-name>
  	<param-value>127.0.0.1</param-value>
  </context-param>
  
  <servlet>
  	<servlet-name>servletEx</servlet-name> 
  	<servlet-class>com.servlet.ServletEx</servlet-class>
  	<init-param>
  		<param-name>adminId</param-name>
  		<param-value>admin</param-value>
  	</init-param>
  	<init-param>
  		<param-name>adminPw</param-name>
  		<param-value>1234</param-value>
  	</init-param>
  </servlet>
   <servlet-mapping>
  	<servlet-name>servletEx</servlet-name>
  	<url-pattern>/se</url-pattern>
  </servlet-mapping>
</web-app>

<!-- 해당 서블릿이 초기화 될 때 Id와 Pw가 생성될 것이다.-->
```

ServletEx.java
```java
package com.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


public class ServletEx extends HttpServlet {
	
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	
		String adminId = getServletConfig().getInitParameter("adminId");
		String adminPw = getServletConfig().getInitParameter("adminPw");
		
		PrintWriter out = response.getWriter();
		out.print("<p>adminId : " + adminId + "</p>");
		out.print("<p>adminPw : " + adminPw + "</p>");
		
		String imgDir = getServletContext().getInitParameter("imgDir");
		String testServerIP = getServletContext().getInitParameter("testServerIP");
		
		out.print("<p>imgDir : " + imgDir + "</p>");
		out.print("<p>adminPw : " + testServerIP + "</p>");
		
		getServletContext().setAttribute("connectedIP", "165.62.58.33");
		getServletContext().setAttribute("connectedUser", "gildong");
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
	}

}

```

ServletGet.java 
```java
package com.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet("/seg")
public class ServletGet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		String connectedIP =(String)getServletContext().getAttribute("connectedIP");
		String connectedUser =(String)getServletContext().getAttribute("connectedUser");
		
		PrintWriter out =response.getWriter();
		out.print("<p>connectedIP : " + connectedIP + "</p>");
		out.print("<p>connectedUser : " + connectedUser + "</p>");
	}

	
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		doGet(request, response);
	}

}
```
  
끝-!😋