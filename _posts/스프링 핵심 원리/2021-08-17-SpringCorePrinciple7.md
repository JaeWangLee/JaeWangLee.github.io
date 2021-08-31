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
  
## 7.2. 옵션 처리
  
주입할 스프링 빈이 없어도 동작해야 할 때가 있다.  
그런데 `@Autowired`만 사용하면 `required` 옵션의 기본 값이 `true`라 <u>자동 주입 대상이 없으면 오류가 발생한다.</u>  
  
자동 주입 대상을 옵션으로 처리하는 방법은 다음과 같다.  
- `@Autowired(required=false)` : 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출이 안됨
- `org.springframework.lang.@Nullable` : 자동 주입할 대상이 없으면 Null이 입력된다.
- `Optional<>` : 자동 주입할 대상이 없으면 `Optional.empty`가 입력된다.  
  
```java
//1.호출 안됨
@Autowired(required = false)
public void setNoBean1(Member member) {
  System.out.println("setNoBean1 = " + member);
}

//2.null 호출
@Autowired
public void setNoBean2(@Nullable Member member) {
  System.out.println("setNoBean2 = " + member);
}

//3.Optional.empty 호출
@Autowired(required = false)
public void setNoBean3(Optional<Member> member) {
  System.out.println("setNoBean3 = " + member);
}
```  
- **Member는 스프링 빈이 아니다.**  
- `setNoBean1()`은 `@Autowired(required=false)`이므로 호출 자체가 안된다.
  
**출력 결과**  
```java
setNoBean2 = null
setNoBean3 = Optional.empty
```  
  
## 7.3. 생성자 주입을 선택해라!
  
과거에는 수정자 주입과 필드 주입을 많이 사용했지만,  
최근에는 스프링을 포함한 DI 프레임워크 대부분이 <u>생성자 주입을 권장</u>한다.
  
**불변**  
- 대부분의 의존관계 주입은 한번 일어나면 애플리케이션 종료시점까지 의존관계를 변경할 일이 없다.
- 대부분의 의존관계는 애플리케이션 종료 전까지 변하면 안된다. 
- 수정자 주입을 사용하면, setXxx 메서드를 public으로 열어두어야 한다.  
- 누군가 실수로 변경할 수도 있고, 변경하면 안되는 메서드를 열어두는 것은 좋은 설계가 아니다.
- 생성자 주입은 객체 생성시 딱 1번만 호출되며 불변하게 설계할 수 있다.  
  
**누락**  
프레임워크 없이 순수한 자바 코드를 단위 테스트 하는 경우에  
다음과 같이 수정자 의존관계인 경우  
```java
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
  //...
}
```  
- `@Autowired`가 프레임워크 안에서 동작할 때는 의존관계가 없으면 오류가 발생하지만,  
- 지금은 프레임워크 없이 순수한 자바 코드로만 단위 테스트를 수행하고 있다.  
  
이렇게 테스트를 수행하면 실행은 된다.
```java
@Test
void createOrder() {
    OrderServiceImpl orderService = new OrderServiceImpl();
    orderService.createOrder(1L, "itemA", 10000);
}
```  
그런데 막상 실행결과는 NPE(Null Point Exception)이 발생하는데,  
memberRepository, discountPolicy 모두 의존관계 주입이 누락되었기 때문이다.  
  
생성자 주입을 사용하면 다음처럼 주입 데이터를 누락 했을 때 **컴파일 오류**가 발생한다.  
그리고 IDE에서 바로 어떤 값을 필수로 주입해야 하는지 알 수 있다.  
  
```java
@Test
void createOrder() {
  OrderServiceImpl orderService = new OrderServiceImpl();
  orderService.createOrder(1L, "itemA", 10000);
}
```  
  
**final 키워드**  
생성자 주입을 사용하면 필드에 `final`키워드를 사용할 수 있다.  
그래서 생성자에서 혹시라도 값이 설정되지 않는 오류를 컴파일 시점에 막아준다.  
```java
@Component
public class OrderServiceImpl implements OrderService {
  private final MemberRepository memberRepository;
  private final DiscountPolicy discountPolicy;
  
  @Autowired
  public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
      this.memberRepository = memberRepository;
  }
//...
}
```  
- 잘 보면 필수 필드인 discountPolicy 에 값을 설정해야 하는데, 이 부분이 누락되었다. 
- 자바는 컴파일 시점에 다음 오류를 발생시킨다.  
  `java: variable discountPolicy might not have been initialized`
- 기억하자! **컴파일 오류는 세상에서 가장 빠르고, 좋은 오류다!**
  
> 참고: 수정자 주입을 포함한 나머지 주입 방식은 모두 생성자 이후에 호출되므로, 필드에 final 키워드를 사용할 수 없다.  
> 오직 생성자 주입 방식만 final 키워드를 사용할 수 있다.
  
**정리**
- 생성자 주입 방식을 선택하는 이유는 여러가지가 있지만, 프레임워크에 의존하지 않고, 순수한 자바 언어의 특징을 잘 살리는 방법이기도 하다.
- 기본으로 생성자 주입을 사용하고, 필수 값이 아닌 경우에는 수정자 주입 방식을 옵션으로 부여하면 된다. 생성자 주입과 수정자 주입을 동시에 사용할 수 있다.
- 항상 생성자 주입을 선택해라! 그리고 가끔 옵션이 필요하면 수정자 주입을 선택해라. 필드 주입은 사용하지 않는게 좋다.
  
## 7.4. 롬복과 최신 트렌드
  
막상 개발을 해보면, 대부분이 다 불면이라 생성자에 final 키워드를 사용하게 된다.  
- 그런데 문제는 생성자도 만들고, 주입 받은 값을 대입해야하고.. 귀찮다.
  
롬복을 적용해서 다음의 기본 코드를 최적화 해보자  
- 롬복 라이브러리가 제공하는 `@RequiredArgsConstructor`기능을 사용하면 final이 붙은 필드를 모아 생성자를 자동으로 만들어준다.
  
**기본 코드**
  
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
  
**최종 결과 코드**  
  
```java
  @Component
  @RequiredArgsConstructor
  public class OrderServiceImpl implements OrderService {
    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;
}
```  
  
이 최종결과 코드와 이전의 코드는 완전히 동일하다.  
롬복이 자바의 애노테이션 프로세서라는 기능을 이용해 컴파일 시점에서 생성자 코드를 자동으로 생성한다.  
실제 class를 열어보면 다음과 같은 코드가 추가되어 있는 것을 확인할 수 있다.  
  
```java
public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy
  discountPolicy) {
      this.memberRepository = memberRepository;
      this.discountPolicy = discountPolicy;
  }
```
  
> **정리**  
> 최근에는 생성자를 딱 1개 두고, `@Autowired`를 생략하는 방법을 주로 사용한다.  
> 여기에 Lombok 라이브러리의 `@RequiredArgsConstructor`를 함께 사용하면 기능은 다 제공하면서, 코드를 깔끔하게 사용할 수 있다.  
  
## 7.5 조회 빈이 2개 이상인 경우
  
`@Autowired`는 타입(Type)으로 조회한다.  
```java
@Autowired
private DiscountPolicy discountPolicy
```  
  
타입으로 조회하기 때문에, 마치 다음 코드와 유사하게 동작한다.(실제로는 더 많은 기능을 제공함!)  
`ac.getBean(DiscountPolicy.class)`  
  
스프링 빈 조회에서 학습했듯이 타입으로 조회하면 선택된 2개 이상일 때 문제가 발생한다.  
`DiscountPolicy`의 하위 타입인 `FixDiscountPolicy`, `RateDiscountPolicy` 둘다 스프링 빈으로 설정해보자.  
  
그러면.. `NoUniqueBeanDefinitionException`오류가 발생한다.  
```
  NoUniqueBeanDefinitionException: No qualifying bean of type
  'hello.core.discount.DiscountPolicy' available: expected single matching bean
  but found 2: fixDiscountPolicy,rateDiscountPolicy
```  
위와 같이 2개의 빈이 발견되었다고 알려준다.  
이때 하위 타입 지정하여 해결할 수 있으나, DIP 위반 및 유연성이 떨어진다.  
따라서 다른 해결방법에 대해 알아보자.  
  
  


끝-!😋