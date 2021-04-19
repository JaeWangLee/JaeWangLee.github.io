---
title: "[강의노트] 예외처리"
excerpt: "Java 예외처리에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - class
  - 예외처리
  - 객체지향
  - 인프런
last_modified_at: 2021-04-19 22:00:20
---

# 26. 예외처리
<span style="color:grey">[자바 프로그래밍 입문 강좌 (renew ver.) - 초보부터 개발자 취업까지!!]내용입니다.</span>
  
프로그램에 문제가 발생했을 때 시스템 동작에 문제가 없도록 사전에 예방하는 코드를 작성하는 방법에 대해 알아보자.  
  
## 26-1. 예외란?
  
![이미지](/assets/images/JAVA/Exceptions/ex1.png)  
  
## 26-2. Exception 클래스
  
![이미지](/assets/images/JAVA/Exceptions/ex2.png)  
  
> Exception 클래스가 전부 다 포함하므로, 가볍게 보고 넘어가자  
  
## 26-3. try ~ catch
  
![이미지](/assets/images/JAVA/Exceptions/ex3.png)  
  
> 자바에선 throws, try ~ catch 두가지가 많이 쓰인다. 
  
``` java
package testPjt

public class MainClass{
    
    public static void main(String[] args){
        int i = 10;
        int j = 0;
        int r = 0;

        //System.out.println("Exception BEFORE");
        //r = i / j; 예외 발생함!

        try{
            r = i / j;
        }catch(Exception e){
            e.printStackTrace(); // 에러 발생 추적
            String msg = e.getMessage(); // 간단하게 사유
            System.out.println(msg);
        }
        System.out.println("Exception AFTER");
    }
}
```
  
## 26-4. 다양한 예외처리  
![이미지](/assets/images/JAVA/Exceptions/ex4.png)  

## 26-5. finally
![이미지](/assets/images/JAVA/Exceptions/ex5.png)  
> 반듯이라고 자꾸 나오는데 내가 적은게 아니다..  
> 반드시가 맞다...

## 26-6. throws
![이미지](/assets/images/JAVA/Exceptions/ex6.png)  
본인을 호출한 메서드로 넘어간다.  

끝-!😋