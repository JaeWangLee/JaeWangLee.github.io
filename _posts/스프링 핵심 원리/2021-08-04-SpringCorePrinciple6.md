---
title: "[스프링 핵심 원리 - 기본편] 섹션6. 컴포넌트 스캔"
published: false
excerpt: "스프링 입문 - 김영한 님의 강의 내용입니다."
toc: true
toc_sticky: true
categories:
  - Spring
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
  
## 6.2. 탐색 위치와 기본 스캔 대상
  
**탐색할 패키지의 시작 위치 지정**  
모든 자바 클래스를 다 컴포넌트 스캔하면 시간이 오래 걸린다. 그래서 꼭 필요한 위치부터 탐색하도록 시작 위치를 지정할 수 있다.  
```java
  @ComponentScan(
    basePackages = "hello.core",
    excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = Configuration.class)
  )
```  
- `basePackages`:탐색할 패키지의 시작 위치를 지정한다.
  - 이 패키지를 포함하여 하위 패키지를 모두 탐색한다.
  - ``basePackages = {"hello.core", "hello.service"}` 이렇게 시작 위치를 지정할 수도 있다.
- `basePackageClasses`: 지정한 클래스의 패키지의 탐색 시작 위치로 지정한다.
- 만약, 지정하지 않을 경우 `@ComponentScan`이 붙은 설정 정보 클래스의 패키지가 시작 위치가 된다.  
  
**권장하는 방법**  
<u>패키지 위치를 지정하지 않고, 설정 정보 클래스의 위치를 프로젝트 최상단에 두는 것</u>이다.  
최근 스프링 부트도 이 방법을 기본으로 제공한다.  
  
예를 들어, 프로젝트가 다음과 같이 구조를 이룬다면  
- com.hello
- com.hello.service
- com.hello.repository
`com.hello` ➡️ 프로젝트 시작 루트, 여기에 `AppConfig` 같은 메인 설정 정보를 두고,  
`@ComponentScan` 애노테이션을 붙이고, `basePackage`지정은 생략한다.  
  
이렇게 하면 `com.hello`를 포함한 하위는 모두 자동으로 컴포넌트 스캔의 대상이 된다.  
그리고 프로젝트 메인 설정 정보는 프로젝트를 대표하는 정보이기 때문에 시작루트에 두는 것이 좋다.  
> 참고  
> 스프링 부트를 사용하면 스프링 부트의 대표 시작 정보인 `@SpringBootApplication`를 이 프로젝트 시작 루트에 두는 것이 관례이다.  
> 그리고 이 설정안에 바로 `@ComponentScan`이 들어있다!  
  
**컴포넌트 스캔 기본 대상**  
컴포넌트 스캔은 `@Component`뿐만 아니라 다음 내용도 추가로 대상에 포함한다.  
또한 아래의 애노테이션은 스프링이 부가기능을 수행하기도 한다.  
- `@Component`
  - 컴포넌트 스캔에서 사용
- `@Controller`
  - 스프링 MVC 컨트롤러로써 스프링은 MVC 컨트롤러로 인식함
- `@Service`
  - 스프링 비즈니스 로직으로써 핵심 비즈니스 계층을 인식하는데 도움을 줌
- `@Repository`
  - 스프링 데이터 접근 계층에서 사용되며, 스프링은 데이터계층의 예외를 스프링 예외로 변환해준다.
- `@Configuration`
  - 스프링 설정 정보에서 사용되며, 스프링은 싱글톤을 유지하도록 추가적인 처리를 해준다.
> 해당 클래스의 소스코드를 보면 `@Component`를 포함하고 있음을 알 수 있다.
  
## 6.3. 필터
  
- `includeFilters` : 컴포넌트 스캔 대상을 추가로 지정한다.
- `excludeFilters` : 컴포넌트 스캔에서 제외할 대상을 지정한다.
  
**컴포넌트 스캔 대상에 추가할 애노테이션**  
```java
  package hello.core.scan.filter;
  import java.lang.annotation.*;
  @Target(ElementType.TYPE)
  @Retention(RetentionPolicy.RUNTIME)
  @Documented
  public @interface MyIncludeComponent {
  }
```
  
**컴포넌트 스캔 대상에서 제외할 애노테이션**  
```java
  package hello.core.scan.filter;
  import java.lang.annotation.*;
  @Target(ElementType.TYPE)
  @Retention(RetentionPolicy.RUNTIME)
  @Documented
  public @interface MyExcludeComponent {
  }
```
  
**컴포넌트 스캔 대상에 추가할 클래스**  
```java
  package hello.core.scan.filter;
  @MyIncludeComponent
  public class BeanA {
  }
```  
  - `@MyIncludeComponent` 적용하였음
  
**컴포넌트 스캔 대상에서 제외할 클래스**  
```java
  package hello.core.scan.filter;
  @MyExcludeComponent
  public class BeanB {
  }
```  
  - `@MyExcludeComponent` 적용하였음
  
**설정 정보와 전체 테스트 코드**  
```java
  package hello.core.scan.filter;
  import org.junit.jupiter.api.Assertions;
  import org.junit.jupiter.api.Test;
  import org.springframework.beans.factory.NoSuchBeanDefinitionException;
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.annotation.AnnotationConfigApplicationContext;
  import org.springframework.context.annotation.ComponentScan;
  import org.springframework.context.annotation.Configuration;
  import org.springframework.context.annotation.FilterType;
  import static org.assertj.core.api.Assertions.assertThat;
  import static org.springframework.context.annotation.ComponentScan.Filter;
  public class ComponentFilerAppConfigTest {

    @Test
    void filterScan() {
      ApplicationContext ac = new AnnotationConfigApplicationContext(ComponentFilterAppConfig.class);
      BeanA beanA = ac.getBean("beanA", BeanA.class);
      assertThat(beanA).isNotNull();
      Assertions.assertThrows(
              NoSuchBeanDefinitionException.class,
              () -> ac.getBean("beanB", BeanB.class));
    }

    @Configuration
    @ComponentScan(
            includeFilters = @Filter(type = FilterType.ANNOTATION, classes = MyIncludeComponent.class),
            excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = MyExcludeComponent.class)
    )
    static class ComponentFilterAppConfig {
    }
```  
  
```java
  @ComponentScan(
    includeFilters = @Filter(type = FilterType.ANNOTATION, classes = MyIncludeComponent.class),
    excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = MyExcludeComponent.class)
  )
```  
  - `includeFilter`에 `MyIncludeComponent` 애노테이션을 추가해서 BeanA가 스프링 빈에 등록된다.
  - `excludeFilter`에 `MyExcludeComponent` 애노테이션을 추가해서 BeanB는 스프링 빈에 등록되지 않는다.
  
#### FilterType 옵션
  
FilterType은 총 5가지가 있다.  
- ANNOTATION : 기본값, 애노테이션을 인식해서 동작한다.
  - ex) `org.example.SomeAnnotation`
- ASSIGNABLE_TYPE : 지정한 타입과 자식 타입을 인식해서 동작한다. 
  - ex) `org.example.SomeClass`
- ASPECTJ: AspectJ 패턴 사용
  - ex) `org.example..*Service+`
- REGEX: 정규 표현식
  - ex) `org\.example\.Default.*`
- CUSTOM: TypeFilter 이라는 인터페이스를 구현해서 처리 
  - ex) `org.example.MyTypeFilter`
  
예를 들어서 BeanA도 빼고 싶으면 다음과 같이 추가하면 된다.  
```java
  @ComponentScan(
    includeFilters = { 
      @Filter(type = FilterType.ANNOTATION, classes = MyIncludeComponent.class),
     },
    excludeFilters = {
          @Filter(type = FilterType.ANNOTATION, classes = MyExcludeComponent.class),
          @Filter(type = FilterType.ASSIGNABLE_TYPE, classes = BeanA.class)
    }
  )
```  
> **참고**  
> `@Component`면 충분하기 때문에, `includeFilters`를 사용할 일은 거의 없다.   
> `excludeFilters`는 여러가지 이유로 간혹 사용할 때가 있지만 많지는 않다.  
> 특히 최근 스프링 부트는 컴포넌트 스캔을 기본으로 제공하는데,  
> 옵션을 변경하면서 사용하기 보다는 스프링의 기본 설정에 최대한 맞추어 사용하는 것을 권장하고, 선호하는 편이다.
  
## 6.4. 중복 등록과 충돌
  
두가지 Case에서 같은 이름이 등록될 수 있다.  
1. 자동 빈 등록 vs 자동 빈 등록
2. 수동 빈 등록 vs 수동 빈 등록
  
**자동 빈 등록 vs 자동 빈 등록**  
- 컴포넌트 스캔에 의해 자동으로 스프링 빈이 등록되는데, 그 이름이 같은 경우 스프링은 오류를 발생시킨다.
  - `ConflictingBeanDefinitionException` 예외 발생
  
**수동 빈 등록 vs 자동 빈 등록**
 - 이 경우에는 수동 빈 등록이 우선권을 갖고 자동 빈을 오버라이딩 해버린다.
  
```java
  @Component
    public class MemoryMemberRepository implements MemberRepository {}
```  
  
```java
  @Configuration
  @ComponentScan(
    excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = Configuration.class)
    )
  public class AutoAppConfig {
    @Bean(name = "memoryMemberRepository")
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
  }
```  
  
**수동 빈 등록 시 남는 로그**
```
  Overriding bean definition for bean 'memoryMemberRepository' with a different 
  definition: replacing
```
  
- 수동이 우선권을 갖는 게 좋을 수 있지만, 의도하지 않았다면 정말 잡기 어려운 버그가 발생한다.
- 그래서 스프링 부트에서는 수동과 자동의 충돌이 나면 오류가 발생하도록 기본값이 설정되있다. 
  
끝-!😋