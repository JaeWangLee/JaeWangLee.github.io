---
title: "[스프링 핵심 원리 - 기본편] 섹션2. 스프링 핵심 원리 이해1 - 예제 만들기"
excerpt: "스프링 입문 - 김영한 님의 강의 내용입니다."
toc: true
toc_sticky: true
categories:
  - Test
tags:
  - Backend
  - Java
  - 김영한
last_modified_at: 2021-07-20 14:30:20
---

# 섹션2. 스프링 핵심 원리 이해1 - 예제 만들기
  
<span style="color:grey">[스프링 핵심 원리 - 기본편] 내용입니다.</span>  
  
## 2.1. 프로젝트 생성
  
1. [스프링 부트 스타터 사이트](https://start.spring.io)로 이동하여 스프링 프로젝트 생성  
![이미지](/assets/images/Spring/스프링_핵심_원리/섹션2/1.png)  
  
2. 압축 해제 후 `build.gradle` 실행  
  
3. `CoreApplication` 실행
  
4. 빠른 실행을 위한 설정 변경
  - `Preferences` - `Build` - `Gradle` 접속
  - Build and run using : IntelliJ IDEA
  - Run tests using : IntelliJ IDEA
  - 이렇게 셋팅해야 IntelliJ에서 Java를 바로 실행하기 때문에 더 빠름
  
## 2.2. 비즈니스 요구사항과 설계
  
- 회원 🙋🏻‍♂️
  - 가입과 조회가 가능해야함
  - 등급이 존재. 일반과 VIP
  - 자체 DB를 구축할 수도, 외부 시스템과 연동할 수 있다.(미확정)
  
- 주문과 할인 정책 🚚
  - 회원은 상품을 주문할 수 있다.
  - 등급에 따라 할인 정책을 달리 적용할 수 있다.
  - 모든 VIP는 1000원 고정할인 적용(나중에 변경될 수 있다.)
  - 할인 정책은 변경 가능성이 매우 높음. 최악의 경우에는 적용하지 않을 수 있음(미확정)
  
당장 결정할 수 없는 부분이 많아 미확정된 부분이 많다.  
**즉, 인터페이스를 만들고 구현체를 언제든지 갈아끼울 수 있도록 설계가 필요하다!**
  
## 2.3. 회원 도메인 설계 
  
- 회원 도메인 협력 관계  
![이미지](/assets/images/Spring/스프링_핵심_원리/섹션2/2.png)  
  - 회원 저장소는 아직 `미확정`이기 때문에 인터페이스를 만든다
  
- 회원 클래스 다이어그램  
![이미지](/assets/images/Spring/스프링_핵심_원리/섹션2/3.png)  
  - 구현 레벨로 내려왔을 때의 다이어그램
  - 회원 서비스를 `MemberService`라는 역할을 인터페이스로 만들고,
  - 그것에 대한 구현체로 `MemberServiceImpl`를 만든다.
  - 회원 저장소의 인터페이스를 `MemberRepository`로 만들고,
  - 그것에 대한 구현체로 `MemoryMemberRepository`,와 `DbMemberRepository`를 만든다.
  
- 회원 객체 다이어 그램
![이미지](/assets/images/Spring/스프링_핵심_원리/섹션2/4.png)  
  - 실제 객체 간의 메모리 참조는 위와 같이 이뤄진다.  
  - 회원 서비스 : `MemberServiceImpl`
  
## 2.4. 회원 도메인 개발
  
### 회원 엔티티
- 회원 등급
  - member라는 패키지안에 `enum`으로 Grade 선언
    <details>
      <summary>코드 보기</summary>  
      <div markdown = "1">
      
          package hello.core.member;
            public enum Grade {
                BASIC,
                VIP 
            }  
        
      </div>
    </details>
  
- 회원 엔티티
  - id, name, grade 등의 엔티티를 설정한다.
  - 생성자 및 getter, setter 구현한다.
     <details>
      <summary>코드 보기</summary>  
      <div markdown = "1">
          ```java
              package hello.core.member;

              public class Member {

                private Long id;
                private String name;
                private Grade grade;

                public Member(Long id, String name, Grade grade) {
                    this.id = id;
                    this.name = name;
                    this.grade = grade;
                }

                public Long getId() {
                    return id;
                }

                public void setId(Long id) {
                    this.id = id;
                }

                public String getName() {
                    return name;
                }

                public void setName(String name) {
                    this.name = name;
                }

                public Grade getGrade() {
                    return grade;
                }

                public void setGrade(Grade grade) {
                    this.grade = grade;
                }
              }  
          ```
      </div>
    </details>
  
### 회원 저장소
- 회원 저장소 인터페이스
  - 요구사항의 `저장`과 `조회`를 구현.
    <details>
      <summary>코드 보기</summary>  
      <div markdown = "1">
          ```java  
          
          ```
      </div>
    </details>

## 2.5. 회원 도메인 실행과 테스트

## 2.6. 주문과 할인 도메인 설계

## 2.7. 주문과 할인 도메인 개발

## 2.8. 주문과 할인 도메인 실행과 테스트
  
끝-!😋