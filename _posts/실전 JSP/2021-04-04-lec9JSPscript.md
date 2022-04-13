---
title: "[실전 JSP] 9. JSP 스크립트"
published: false
excerpt: "JSP 스크립트에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - JSP
tags:
  - JSP
  - Servlet
  - Script
  - 웹프로그램
last_modified_at: 2021-04-04 13:45:20
---

# 9. JSP 스크립트
<span style="color:grey">[실전 JSP (renew ver.) - 신입 프로그래머를 위한 강좌] 내용입니다.</span> 

## 9-1. Servlet vs JSP
![이미지](/assets/images/JSP&Servlet/실전JSP/9강/9강_1.png)

Servlet은 순수 자바 코드로만 구성되며,  
HttpServlet 클래스를 반드시 상속받아야 합니다.  

JSP는 html과 java를 합쳐서 구성되며,
개발자가 만들어 놓고나면 JVM의 해석을 위해 자바로 해석되어야 합니다.(컨테이너가 처리)  
jsp파일도 java파일과 클래스파일로 만들어지기 때문에 결국에는 Servlet 파일이라고 할 수 있죠. 

## 9-2. JSP 파일 HTML5 포멧 설정
![이미지](/assets/images/JSP&Servlet/실전JSP/9강/9강_2.png)

## 9-3. JSP 주요 스크립트
![이미지](/assets/images/JSP&Servlet/실전JSP/9강/9강_3.png)

![이미지](/assets/images/JSP&Servlet/실전JSP/9강/9강_4.png)

![이미지](/assets/images/JSP&Servlet/실전JSP/9강/9강_5.png)

### 실습

jspEx.jsp
```java
<%@ page import="java.util.ArrayList"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%@ include file = "header.jsp" %>
	
	<!-- 선언 태그 -->
	<%!
		int num = 10;
		String str = "jsp";
		ArrayList<String> list = new ArrayList<String>();
		
		public void jspMethod(){
			System.out.println("-- jspMethod() --");
		}
	%>
	<!-- 주석태그 -->
	<!-- 여기는 주석 입니다. -->
	<%-- Hello JSP World! --%>
	
	<!-- 스크립트릿 태그 -->
	<%
		if(num > 0){
	%>
	<p> num > 0 </p>
	<%
		}else{
	%>
	<p> num <= 0 </p>
	<%
		}
	%>
	
	<!-- 표현식 태그 -->
	num is <%= num %>
	
	<%@ include file = "footer.jsp" %>
	
</body>
</html>
```

header.jsp
```java
<p> Header Contents </p>
```

footer.jsp
```java
<p> Footer Contents </p>
```

서버 실행 결과
![이미지](/assets/images/JSP&Servlet/실전JSP/9강/9강_6.png)

소스검사
![이미지](/assets/images/JSP&Servlet/실전JSP/9강/9강_7.png)

>jsp 스크립트는 제외된 것을 볼 수 있다!

끝-!😋