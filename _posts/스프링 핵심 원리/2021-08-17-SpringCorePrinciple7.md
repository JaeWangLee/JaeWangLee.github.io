---
title: "[스프링 핵심 원리 - 기본편] 섹션7. 의존관계 자동 주입"
excerpt: "스프링 입문 - 김영한 님의 강의 내용입니다."
toc: true
toc_sticky: true
categories:
  - Spring
tags:
  - Backend
  - Java
  - 김영한
last_modified_at: 2021-08-17 21:00:20
---

# 섹션7. 의존관계 자동 주입
  
<span style="color:grey">[스프링 핵심 원리 - 기본편] 내용입니다.</span>  
  
## 7.1. 다양한 의존관계 주입 방법
  
의존관계 주입은 크게 4가지 방법이 있다.  
- 생성자 주입
- 수정자 주입(Setter 주입)
- 필드 주입
- 일반 메서드 주입
  
**생성자 주입**  
- 이름 그대로 생성자를 통해 의존관계를 주입 받는 방법
- 특징
  - 생성자 호출 시점에 <u>딱 1번만 호출되는 것이 보장</u>된다.
  - **불변, 필수** 의존관계에 사용
  
```java
@Component
public class OrderServiceImpl implements OrderService {
  private final MemberRepository memberRepository;
  private final DiscountPolicy discountPolicy;

  @Autowired
  public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
  }
}
```  
  
> 생성자가 딱 1개만 있다면 `@Autowired`생략해도 자동주입 된다.  
> 물론 스프링 빈에만 해당.  
  
**수정자 주입**  
- setter라 불리는 필드의 값을 변경하는 수정자 메서드를 통해서 의존관계를 주입한다.
- 특징
  - **선택, 변경** 가능성이 있는 의존관계에 사용
  - <u>자바빈 프로퍼티 규약</u>의 수정자 메서드 방식을 사용하는 방법이다. 
  
```java
@Component
public class OrderServiceImpl implements OrderService {
  private MemberRepository memberRepository;
  private DiscountPolicy discountPolicy;

  @Autowired
  public void setMemberRepository(MemberRepository memberRepository) {
      this.memberRepository = memberRepository;
  }

  @Autowired
  public void setDiscountPolicy(DiscountPolicy discountPolicy) {
      this.discountPolicy = discountPolicy;
  }
}
```  
> 참고 : `@Autowired`의 기본 동작은 주입할 대상이 없으면 오류가 발생한다.  
> 주입할 대상이 없어도 동작하게 하려면 `@Autowired(required = false)`로 지정하면 된다.  
  
**필드 주입**  
- 이름 그대로 필드에 바로 주입하는 방법이다.
- 특징
  - 코드가 간결하나, 외부에서 변경이 불가능하여 테스트하기 힘들다는 치명적인 단점이 있다.
  - DI 프레임워크가 없으면 아무것도 할 수 없다.
  - **사용하지 말자!**
    - 애플리케이션의 실제 코드와 관계 없는 테스트 코드
    - 스프링 설정을 목적으로 하는 `@Configuration` 같은 곳에서만 특별한 용도로 사용
  
```java
@Component
public class OrderServiceImpl implements OrderService {
    @Autowired
    private MemberRepository memberRepository;
    @Autowired
    private DiscountPolicy discountPolicy;
}
```  
> 참고 : 순수한 자바 테스트 코드에는 당연히 `@Autowired`가 동작하지 않는다.  
> `@SpringBootTest`처럼 스프링 컨테이너를 테스트에 통합한 경우에만 가능하다.  
  
> 참고 : 다음 코드와 같이 `@Bean`에서 파라미터에 의존관계는 자동 주입된다.  
> 수동 등록 시 자동 등록된 빈의 의존관계가 필요할 때 문제를 해결할 수 있다.  
  
```java
@Bean
OrderService orderService(MemberRepository memberRepoisitory, DiscountPolicy discountPolicy) {
  new OrderServiceImpl(memberRepository, discountPolicy)
}
```  
  
**일반 메서드 주입**  
- 일반 메서드를 통ㄹ해서 주입 받을 수 있다.
- 특징
  - 한번에 여러 필드를 주입 받을 수 있다.  
  - 일반적으로 잘 사용하지 않는다.
  
```java
@Component
public class OrderServiceImpl implements OrderService {
  private MemberRepository memberRepository;
  private DiscountPolicy discountPolicy;

  @Autowired
  public void init(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
      this.memberRepository = memberRepository;
      this.discountPolicy = discountPolicy;
  }
}
```  
> 참고 : 의존관계 자동 주입은 스프링ㄹ 컨테이너가 관리하는 스프링 빈이어야 동작한다.  
> 스프링 빈이 아닌 `Member` 같은 클래스에서 `@Autowired`코드를 적용해도 아무 기능도 동작하지 않는다.  
  

끝-!😋