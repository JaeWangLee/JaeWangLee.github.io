---
title: "[실전 JSP] 7. Servlet Life Cycle"
published: false
excerpt: "Servlet Life Cycle에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - JSP
tags:
  - JSP
  - Servlet
  - 웹프로그램
last_modified_at: 2021-04-04 11:45:20
---

# 7. Servlet Life-Cycle
<span style="color:grey">[실전 JSP (renew ver.) - 신입 프로그래머를 위한 강좌] 내용입니다.</span>
  
## 7-1. Servlet 생명주기
![이미지](/assets/images/JSP&Servlet/실전JSP/7강/7강_1.png)
1. @PostConstruct : 시작하기 전에 준비하는 단계  
2. init( ) : 서블릿의 생성 단계  
3. service : 개발자가 구현한 기능대로 서블릿이 일하는 단계  
4. destroy( ) : 서블릿이 소멸되는 단계  
5. @PreDestroy : 서블릿 종료 후 서블릿을 정리하기 위한 단계  

> Servlet 객체는 최초 1번 생성되며, 요청때마다 생성되지 않고 재활용한다.  
> Init( ) 호출과 Destory도 각각 최초, 마지막 1번 진행된다.  
> Service(doGet( ), doPost( ) 등)는 매번 진행된다.  
  
## 7-2. 생명주기 관련 메서드
![이미지](/assets/images/JSP&Servlet/실전JSP/7강/7강_2.png)
단계별로 콜백(호출) 메서드를 제공함으로써 특정한 작업을 더할 수 있음.  

### 실습

```java
package com.servlet;

import java.io.IOException;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/tsc")
public class TestServletClass extends HttpServlet {
	private static final long serialVersionUID = 1L;
   
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().append("Served at: ").append(request.getContextPath());
		System.out.println("--- doGet() ---");
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}
	
	@PostConstruct
	private void funPC() {
		System.out.println("-- @PostConstruct --");

	}
	
	@Override
	public void init() throws ServletException {
		System.out.println("--- init() ---");
	}
	
	@Override
	public void destroy() {
		System.out.println("--- destroy() ---");
	}
	
	@PreDestroy
	private void funPd() {
		System.out.println("-- @PreDestroy --");

	}

}
```
![이미지](/assets/images/JSP&Servlet/실전JSP/7강/7강_3.png)
  
끝-! 😋