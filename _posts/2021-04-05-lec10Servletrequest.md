---
title: "[강의노트] JSP request, response"
excerpt: "JSP request, response에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - JSP
tags:
  - JSP
  - Servlet
  - 웹프로그램
last_modified_at: 2021-04-05 20:36:20
---
# 10. JSP request, response
<span style="color:grey">[실전 JSP (renew ver.) - 신입 프로그래머를 위한 강좌] 내용입니다.</span>

서블릿과의 차이점은 서블릿에서 사용하냐, jsp에서 사용하냐의 차이일뿐,  
같은 객체입니다. <u>즉, 제공해주는 메소드나 용도가 같습니다.</u>  
  
## 10-1. request 객체

![이미지](/assets/images/JSP&Servlet/실전JSP/10강/10강_1.png)

폼을 이용해서 사용자의 데이터를 받고, 어떠한 jsp로 넘길지 액션에 명시해줍니다.  

## 10-2. response 객체

![이미지](/assets/images/JSP&Servlet/실전JSP/10강/10강_2.png)

## 10-3. 실습

💁🏻‍♂️ Request 관련
formEx.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<form action = "mSignUp.jsp" method = "get">
		name : <input type = "text" name = "m_name"><br>
		password : <input type = "password" name = "m_pass"><br>
		hobby : sport<input type = "checkbox" name = "m_hobby" value ="sport">,
				cooking<input type = "checkbox" name = "m_hobby" value ="cooking">,
				travel<input type = "checkbox" name = "m_hobby" value ="travel"><br>
				<input type = "submit" value = "sign up">
	</form>
</body>
</html>
```

mSignUp.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%!
		String m_name;
		String m_pass;
		String[] m_hobby;
	%>
	
	<%
	m_name = request.getParameter("m_name");
	m_pass = request.getParameter("m_pass");
	m_hobby = request.getParameterValues("m_hobby");
	%>
	
	m_name : <%= m_name %> <br>
	m_pass : <%= m_pass %> <br>
	m_hobby :
	<%
	for(int i = 0; i < m_hobby.length; i++){
	%>
		<%= m_hobby[i] %>
	<%
	}
	%><br>
	
	
</body>
</html>
```

💁🏻‍♂️ Response 관련

firstPage.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<p>first Page!!</p>
	
	<%
	response.sendRedirect("secondPage.jsp");
	%>
</body>
</html>
```

secondPage.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<p>second Page!!</p>
</body>
</html>
```
  
끝-!