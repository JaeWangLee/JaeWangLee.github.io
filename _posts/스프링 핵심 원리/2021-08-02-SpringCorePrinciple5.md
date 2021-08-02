---
title: "[스프링 핵심 원리 - 기본편] 섹션5. 싱글톤 컨테이너"
excerpt: "스프링 입문 - 김영한 님의 강의 내용입니다."
toc: true
toc_sticky: true
categories:
  - Test
tags:
  - Backend
  - Java
  - 김영한
last_modified_at: 2021-08-02 19:00:20
---

# 섹션5. 싱글톤 컨테이너
  
<span style="color:grey">[스프링 핵심 원리 - 기본편] 내용입니다.</span>  
  
## 5.1. 웹 애플리케이션과 싱글톤
  
- 스프링은 태생이 기업용 온라인 서비스 기술을 지원하기 위해 탄생했다.
- 대부분의 스프링 애플리케이션은 웹 애플리케이션이며, <u>보통 여러 고객이 동시에 요청을 한다. </u>
![이미지](/assets/images/Spring/스프링_핵심_원리/섹션5/1.png)  
  
- 스프링 없는 순수한 DI 컨테이너를 만들어서 테스트 해보자.  
  ```java
  package hello.core.singleton;

  import hello.core.AppConfig;
  import hello.core.member.MemberService;
  import org.assertj.core.api.Assertions;
  import org.junit.jupiter.api.DisplayName;
  import org.junit.jupiter.api.Test;

  public class SingletonTest {

      @Test
      @DisplayName("스프링 없는 순수한 DI 컨테이너")
      void pureContainer(){
          AppConfig appConfig = new AppConfig();
          //1. 조회 : 호출할 때 마다 객체 생성
          MemberService memberService1 = appConfig.memberService();

          //2. 조회 : 호출할 때 마다 객체 생성
          MemberService memberService2 = appConfig.memberService();

          //참조값이 다른 것을 확인
          System.out.println("memberService1 = " + memberService1);
          System.out.println("memberService2 = " + memberService2);

          Assertions.assertThat(memberService1).isNotSameAs(memberService2);
      }
  }
  ```  
  
- 결과  
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션5/2.png)  
  - 우리가 만들었던 스프링 없는 순수한 DI 컨테이너인 AppConfig는 <u>요청을 할 때마다 객체를 새로 생성한다.</u>
  - 고객 트래픽이 초당 100이 나오면 초당 100개에 객체가 생성되고 소멸한다! ➡️ **메모리 낭비가 심함!**
  - 해결방안은 **해당 객체가 딱 1개만 생성되고, 공유하도록 설계하면 된다. ➡️ 싱글톤 패턴**💡
  
## 5.2. 싱글톤 패턴
  

끝-!😋