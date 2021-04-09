---
title: "[강의노트] form 데이터 처리"
excerpt: "Servlet form에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - JSP
tags:
  - JSP
  - Servlet
  - form
  - 웹프로그램
last_modified_at: 2021-04-04 12:45:20
---

# 8. form 데이터 처리
<span style="color:grey">[실전 JSP (renew ver.) - 신입 프로그래머를 위한 강좌] 내용입니다.</span>

## 8-1. form 태그
![이미지](/assets/images/JSP&Servlet/실전JSP/8강/8강_1.png)
**form**이란 클라이언트가 로긴, 회원가입, 설문지 등의 정보를 입력하고 서버로 제출하기 기구입니다.  
Submit을 통해 데이터가 웹 컨테이너로 날라가는데 이때 리퀘스트 객체에 묶여서 전송이 됩니다.  
서버에서 리퀘스트 객체를 받게 되는데 이때 받는 메서드는 **doGet과 doPost**이다.  
  
## 8-2. doGet( )
![이미지](/assets/images/JSP&Servlet/실전JSP/8강/8강_2.png)
form 태그에서 method 속성을 get으로 설정하면 doGet()메서드로 받게 됩니다.  
> 참고로 method 속성의 디폴트는 'get'이다.  
  
form 태그에 입력한 정보가 URL에 노출되어 전송되기 때문에 보안에 취약하며,  
text길이의 한계가 있어 너무 많은 데이터를 보내기에도 문제가 됩니다.  

## 8-3. doPost( )
![이미지](/assets/images/JSP&Servlet/실전JSP/8강/8강_3.png)
form 태그에서 method 속성을 post로 설정하면 doPost()메서드로 받게 됩니다.  
맵핑 정보만 노출되고,
사용자에 데이터가 헤더파일에 암호화되어 전송되므로 보안에 강한 장점이 있습니다.  
  
끝-!😋
