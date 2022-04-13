---
title: "[실전 JSP] 6. Servlet Request, Response"
published: false
excerpt: "Servlet request, response에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - JSP
tags:
  - JSP
  - Servlet
  - 웹프로그램
last_modified_at: 2021-04-03 22:20:20
---

# 6. Servlet request, response
<span style="color:grey">[실전 JSP (renew ver.) - 신입 프로그래머를 위한 강좌] 내용입니다.</span>

## 6-1. HttpServlet
![이미지](/assets/images/JSP&Servlet/실전JSP/6강/6강_1.png)

통신으로 다양한 데이터가 오고가야 할때는 많은 기능들이 필요합니다.  
자바에는 이러한 기능이 표준화되어 있으며,  
우린 이러한 것들을 상속 받아놓은  **HttpServlet class**를 상속(extends)하여 사용만 하면 됩니다.😊

> ※ HttpServlet을 상속 받는 법
![이미지](/assets/images/JSP&Servlet/실전JSP/6강/6강_2.png)
> 이클립스에서는 Servlet생성시 자동으로 상속받는다. 
  
## 6-2. HttpServletRequest
![이미지](/assets/images/JSP&Servlet/실전JSP/6강/6강_3.png)
- HttpServletRequest : 요청 처리 객체  
ex. 클라이언트 쪽에서 아이디와 패스워드를 입력했다 치면, 해당 데이터가 request 객체에 실어져 전송됨.  
## 6-3. HttpServletResponse
![이미지](/assets/images/JSP&Servlet/실전JSP/6강/6강_4.png)  
- HttpServletResponse : 응답 처리 객체  
 ex. 회원 여부에 대한 정보는 response 객체에 실어서 클라이언트 쪽으로 전송함
   
끝-!😋