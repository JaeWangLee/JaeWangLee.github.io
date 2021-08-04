---
title: "[스프링 핵심 원리 - 기본편] 섹션6. 컴포넌트 스캔"
excerpt: "스프링 입문 - 김영한 님의 강의 내용입니다."
toc: true
toc_sticky: true
categories:
  - Test
tags:
  - Backend
  - Java
  - 김영한
last_modified_at: 2021-08-04 21:00:20
---

# 섹션6. 컴포넌트 스캔
  
<span style="color:grey">[스프링 핵심 원리 - 기본편] 내용입니다.</span>  
  
## 6.1. 컴포넌트 스캔과 의존관계 자동 주입 시작하기
  
지금까지 스프링 빈을 등록할 때는 자바 코드의 @Bean이나 XML의 <bean> 등을 통해서 설정 정보에 직접 등록할 스프링 빈을 나열했다.  
하지만 귀찮다.. 그래서 스프링은 설정 정보가 없어도 자동으로 스프링 빈을 등록하는 <u>컴포넌트 스캔</u>이라는 기능을 제공한다.  
또 <u>의존관계도 자동으로 주입하는</u> `@Autowired` 라는 기능도 제공한다.  
  
코드로 컴포넌트 스캔과 의존관계 자동 주입을 알아보자.  
**AutoAppConfig.java**  
  ```java
    package hello.core;
    import org.springframework.context.annotation.ComponentScan;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.context.annotation.FilterType;
    import static org.springframework.context.annotation.ComponentScan.*;
    @Configuration
    @ComponentScan(
      excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = Configuration.class)
    )
    public class AutoAppConfig {
      
    }
  ```  
  - 컴포넌트 스캔을 사용하려면 `@ComponentScan`을 설정정보에 붙여주면 된다.
  - 기존의 AppConfig와는 다르게 `@Bean`으로 등록한 클래스가 하나도 없다!
  
> **참고**  
> 컴포넌트 스캔을 사용하면 `@Configuration`이 붙은 설정 정보도 자동으로 등록되기 때문에,  
> `AppConfig`, `TestConfig`등 앞서 만들어두었던 설정 정보도 함께 등록되고, 실행되어 버린다.  
> 그래서 `excludeFilters`를 이용해서 설정정보는 컴포넌트 스캔 대상에서 제외했다.  
> 보통 설정 정보를 컴포넌트 스캔 대상에서 제외하지는 않지만, 기존 예제 코드를 최대한 남기고 유지하기 위해서 이 방법을 선택했다.
  
컴포넌트 스캔은 이름 그대로 `@Component` 애노테이션이 붙은 클래스를 스캔해서 스프링 빈으로 등록한다.
이제 각 클래스가 컴포넌트 스캔의 대상이 되도록 `@Component` 애노테이션을 붙여주자.
  
> **참고**  
> `@Configuration`이 컴포넌트 스캔의 대상이 된 이유도 `@Configuration` 소스코드를 열어보면 `@Component` 애노테이션이 붙어있기 때문이다.
  
**MemoryMemberRepository @Component 추가**  
```java
  @Component
  public class MemoryMemberRepository implements MemberRepository {
```  
  
**RateDiscountPolicy @Component 추가**  
```java
  @Component
  public class RateDiscountPolicy implements DiscountPolicy {
```  
  
**MemberServiceImpl @Component, @Autowired 추가**
```java
  @Component
  public class MemberServiceImpl implements MemberService {
      private final MemberRepository memberRepository;
      @Autowired
      public MemberServiceImpl(MemberRepository memberRepository) {
          this.memberRepository = memberRepository;
      }
  }
```
  
- 이전에 AppConfig에서는 `@Bean`으로 직접 설정 정보를 작성했고, 의존관계도 직접 명시했다.
- 하지만, 이런 설정 정보 자체가 없기 때문에 의존관계를 자동으로 주입해주는 `@Autowired`를 사용한다.
  
**OrderServiceImpl @Component, @Autowired 추가**  
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
- `@Autowired`를 사용하면 생성자에게 여러 의존관계도 한번에 주입받을 수 있다.  
  
**AutoAppConfigTest.java**  
```java
  package hello.core.scan;
  import hello.core.AutoAppConfig;
  import hello.core.member.MemberService;
  import org.junit.jupiter.api.Test;
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.annotation.AnnotationConfigApplicationContext;
  import static org.assertj.core.api.Assertions.*;
  public class AutoAppConfigTest {
    @Test
    void basicScan() {
        ApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class);
        MemberService memberService = ac.getBean(MemberService.class);
        assertThat(memberService).isInstanceOf(MemberService.class);
    }
}
```
- `AnnotationConfigApplicationContext`를 사용하는 것은 기존과 동일하다.
- 설정 정보로 `AutoAppConfig`클래스를 넘겨준다.
- 결과는 기존과 동일함을 알 수 있다.  
  
  
그렇다면, 컴포넌트 스캔과 자동 의존관계 주입이 어떻게 동작하는지 알아보자.  
1. **@ComponentScan**  
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션6/1.png)  
  - `@ComponentScan`은 `@Component`가 붙은 모든 클래스를 스프링 빈으로 등록한다.
  - 이때 스프링 빈의 기본 이름은 클래스명을 사용하되 맨 앞글자만 소문자를 사용한다. 
    - 빈 이름 기본 전략 : `MemberServiceImpl`클래스 ➡️ `memberServiceImpl`  
    - 빈 이름 직접 지정 : 만약 스프링 빈의 이름을 직접 지정하고 싶으면 `@Component("memberService2")`이런식으로 이름을 부여하면 된다.
  
2. **@Autowired 의존관계 자동 주입**
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션6/2.png)  
  - 생성자에 `@Autowired`를 지정하면, 스프링 컨테이너가 자동으로 해당 스프링 빈을 찾아서 주입한다.
  - 이때 기본 조회 전략은 타입이 같은 빈을 찾아서 주입한다.
    - `getBean(MemberRepository.class)`와 동일하다고 이해하면 된다.
  
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션6/3.png)  
  - 생성자에 파라미터가 많아도 다 찾아서 자동으로 주입한다.

  
끝-!😋