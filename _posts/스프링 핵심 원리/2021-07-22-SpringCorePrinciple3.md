---
title: "[스프링 핵심 원리 - 기본편] 섹션3. 스프링 핵심 원리 이해2 - 객체 지향 원리 적용"
excerpt: "스프링 입문 - 김영한 님의 강의 내용입니다."
toc: true
toc_sticky: true
categories:
  - Test
tags:
  - Backend
  - Java
  - 김영한
last_modified_at: 2021-07-22 21:30:20
---

# 섹션3. 스프링 핵심 원리 이해2 - 객체 지향 원리 적용
  
<span style="color:grey">[스프링 핵심 원리 - 기본편] 내용입니다.</span>  
  
## 3.1. 새로운 할인 정책 개발
  
새로운 할인 정책을 확장해보자.  
기획자 曰 : 고정식 할인이 아닌, **VIP는 구입 금액의 10%를 할인하도록 변경해주세요.**  
  
- RateDiscountPolicy 추가  
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션3/1.png)  
  <details>
  <summary>코드 보기</summary>
  <div markdown = "1">
    ```java  
    package hello.core.discount;

    import hello.core.member.Grade;
    import hello.core.member.Member;

    public class RateDiscountPolicy implements DiscountPolicy {

        private int discountPercent = 10;

        @Override
        public int discount(Member member, int price) {
            if(member.getGrade() == Grade.VIP){
                return price * discountPercent / 100;
            }else{
                return 0;
            }
        }
    }
    ```
  </div>
  </details>
  
- RateDiscountPolicy <u>Test 작성</u>
  <details>
  <summary>코드 보기</summary>
  <div markdown = "1">
    ```java  
    package hello.core.discount;

    import hello.core.member.Grade;
    import hello.core.member.Member;
    import org.assertj.core.api.Assertions;
    import org.junit.jupiter.api.DisplayName;
    import org.junit.jupiter.api.Test;

    class RateDiscountPolicyTest {
        RateDiscountPolicy discountPolicy = new RateDiscountPolicy();

        @Test
        @DisplayName("VIP는 10% 할인이 적용되어야 한다.")
        void vip_o(){
            //given
            Member member = new Member(1L, "memberVIP", Grade.VIP);
            //when
            int discount = discountPolicy.discount(member, 10000);
            //then
            Assertions.assertThat(discount).isEqualTo(1000);
        }

        @Test
        @DisplayName("VIP가 아니면 10% 할인이 적용되지 않아야 한다.")
        void vip_x(){
            //given
            Member member = new Member(2L, "memberBASIC", Grade.BASIC);
            //when
            int discount = discountPolicy.discount(member, 10000);
            //then
            Assertions.assertThat(discount).isEqualTo(0); // 0원이 적용됨!
        }
    }
    ```
  </div>
  </details>
  
## 3.2. 새로운 할인 정책 적용과 문제점
  
**새로운 할인 정책을 애플리케이션에 적용해보자.**  
할인 정책을 변경하려면 클라이언트인 `OrderServiceImpl` 코드를 고쳐야 한다.  
```java
  public class OrderServiceImpl implements OrderService {
  //    private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
      private final DiscountPolicy discountPolicy = new RateDiscountPolicy();
  }
```
- 문제점 발견
  - 우리는 역할과 구현을 충실하게 분리했다. ⭕️  
  - 다형성도 활용하고, 인터페이스와 구현 객체를 분리했다. ⭕️  
  - **OCP, DIP 같은 객체지향 설계 원칙을 충실히 준수했다.** ❌  
    - **DIP 준수?** ❌  
      추상(인터페이스)뿐만 아니라 <u>구체(구현)클래스</u>에도 의존하고 있다.  
        - 추상(인터페이스) 의존: DiscountPolicy  
        - 구체(구현) 클래스: FixDiscountPolicy , RateDiscountPolicy  
    - **OCP 준수?** ❌  
      지금 코드는 기능을 확장해서 변경하면, <u>클라이언트 코드에 영향을 준다!</u> 따라서 OCP를 위반한다.  
  
**그렇다면 왜 클라이언트 코드를 변경해야 할까?**
  
- 기대했던 의존관계
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션3/2.png)  
  - 지금까지 단순히 `DiscountPolicy` 인터페이스만 의존한다고 생각했다.  
  
- 실제 의존관계
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션3/3.png)  
  - 클라이언인 `OrderServiceImpl`이 `DiscountPolicy` 인터페이스 뿐만 아니라  
    `FixDiscountPolicy`인 구체 클래스도 함께 의존하고있다. **DIP위반!**  
  
- 정책 변경 시 
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션3/4.png)  
  ※ 그래서 `FixDiscountPolicy`를 `RateDiscountPolicy`로 변경하는 순간  
    `OrderServiceImpl`의 소스코드도 함께 변경해야 한다. **OCP위반!**
  
**어떻게 문제를 해결할 수 있을까?**
- 클라이언트 코드인 `OrderServiceImpl` 은 `DiscountPolicy`의 인터페이스 뿐만 아니라 구체 클래스도 함께 의존한다.  
- 그래서 구체 클래스를 변경할 때 클라이언트 코드도 함께 변경해야 한다.
- DIP 위반 추상에만 의존하도록 변경(인터페이스에만 의존)
- DIP를 위반하지 않도록 인터페이스에만 의존하도록 의존관계를 변경하면 된다.
  
- **인터페이스에만 의존하도록 설계 변경**  
![이미지](/assets/images/Spring/스프링_핵심_원리/섹션3/5.png)  
  
- **인터페이스에만 의존하도록 코드 변경**
  ```java
  public class OrderServiceImpl implements OrderService {
      //private final DiscountPolicy discountPolicy = new RateDiscountPolicy();
      private DiscountPolicy discountPolicy;
  }
  ```
  - 인터페이스에만 의존하도록 설계와 코드를 변경했다.
  - 그런데 구현체가 없는데 어떻게 코드를 실행할 수 있을까?
  - 실제 실행을 해보면 NPE(null pointer exception)가 발생한다.
  
이 문제를 해결하려면 누군가 클라이언트인 `OrderServiceImpl`에 `DiscountPolicy`의 구현객체를 대신 생성하고 주입해주어야 한다.  
  
## 3.3. 관심사의 분리
  
**애플리케이션**을 **공연**이라 생각해보자.  
그럼 각각의 **배역**은 **인터페이스**가 될 것이고, 실제 **배우**는 **구현체**가 될 것이다.  
로미오 역할(인터페이스)을 하는 디카프리오(구현체)가 있다면,  
디카프리오는 로미오 역할만 충실하면 될 뿐, 줄리엣에 대해서 관여할 필요가 없다.  
  
그 이유는 공연을 기획하는 **공연 기획자**가 있기 때문인데,  
어플리케이션도 각각의 구현체가 각각의 인터페이스의 역할에 충실할 수 있도록 기획자가 필요하다.  
  
### AppConfig의 등장
어플리케이션 전체 동작 방식을 구성하기 위해, <u>구현 객체를 생성하고, 연결</u>하는 책임을 갖는 별도의 설정 클래스를 만들어 보자.  

- **AppConfig**
    <details>
    <summary>코드 보기</summary>
    <div markdown = "1">
      ```java  
      package hello.core;

      import hello.core.discount.FixDiscountPolicy;
      import hello.core.member.MemberService;
      import hello.core.member.MemberServiceImpl;
      import hello.core.member.MemoryMemberRepository;
      import hello.core.order.OrderService;
      import hello.core.order.OrderServiceImpl;

      public class AppConfig {
          public MemberService memberService(){
              return new MemberServiceImpl(new MemoryMemberRepository());
          }

          public OrderService orderService(){
              return new OrderServiceImpl(
                  new MemoryMemberRepository(),
                  new FixDiscountPolicy());
          }
      }
      ```
    </div>
    </details>  
  
  - `AppConfig`는 애플리케이션의 실제 동작에 필요한 <u>구현 객체를 생성</u>한다.  
    - `MemberServiceImpl`
    - `MemoryMemberRepository`
    - `OrderServiceImple`
    - `FixDiscountPolicy`
  - `AppConfig`는 생성한 객체 인스턴스의 참조(레퍼런스)를 <u>생성자를 통해서 주입(연결)</u>해준다.  
    - `MemoryServiceImpl` ➡️ `MemoryMemberRepository`
    - `OrderServiceImpl` ➡️ `MemoryMemberRepository`, `FixDiscountPolicy`  
  
- **MemberServiceImpl** - 생성자 주입
    <details>
    <summary>코드 보기</summary>
    <div markdown = "1">
      ```java  
      package hello.core.member;

      public class MemberServiceImpl implements MemberService{

          private final MemberRepository memberRepository;

          public MemberServiceImpl(MemberRepository memberRepository){
              this.memberRepository = memberRepository;
          }

          @Override
          public void join(Member member) {
              memberRepository.save(member);
          }

          @Override
          public Member findMember(Long memberId) {
              return memberRepository.findById(memberId);
          }
      }
      ```
    </div>
    </details>  
  
  - 설계 변경으로 `MemberServiceImpl`은 `MemoryMemberRepository`를 의존하지 않는다!  
  - 단지 `MemoryRepository` 인터페이스만 의존한다.  
  - `MemberServiceImpl`입장에서 생성자를 통해 어떤 구현 객체가 들어올지(주입될지)는 알 수 없다.  
  - `MemberServiceImpl`의 생성자를 통해서 어떤 구현 객체를 주입할지는 오직 외부(`AppConfig`)에서 결정된다.  
  - `MemberServiceImpl`은 이제부터 <u>의존관계에 대한 고민은 외부에 맡기고 실행에만 집중</u>하면 된다.  
  
- 클래스 다이어그램
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션3/6.png)  
  - 객체의 생성과 연결은 `AppConfig`가 담당한다.
  - **DIP 완성** : `MemberServiceImpl`은 `MemberRepository`인 추상에만 의존하면 된다. 
  - **관심사의 분리** : 객체를 생성하고 연결하는 역할과 실행하는 역할이 명확히 분리되었다.  
  

- 회원 객체 인스턴스 다이어그램
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션3/7.png)  
  - `appConfig`객체는 `memoryMemberRepository` 객체를 생성하고 그 참조값을 `memberServiceImpl`을 생성하면서 생성자로 전달한다.  
  - 클라이언트인 `memberServiceImpl`입장에서 보면 의존관계를 마치 외부에서 주입해주는 것과 같다고 해서 **DI**(Dependency Injection) 우리말로 **의존관계 주입** 또는 **의존성 주입**이라 한다.  
  
- **OrderServiceImpl** - 생성자 주입
  
    <details>
    <summary>코드 보기</summary>
    <div markdown = "1">
      ```java  
      package hello.core.order;

      import hello.core.discount.DiscountPolicy;
      import hello.core.member.Member;
      import hello.core.member.MemberRepository;

      public class OrderServiceImpl implements OrderService{

          private final MemberRepository memberRepository;
          private final DiscountPolicy discountPolicy;

          public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy){
              this.memberRepository = memberRepository;
              this.discountPolicy = discountPolicy;
          }

          @Override
          public Order createOrder(Long memberId, String itemName, int itemPrice) {
              Member member = memberRepository.findById(memberId);
              int discountPrice = discountPolicy.discount(member, itemPrice);

              return new Order(memberId, itemName, itemPrice, discountPrice);
          }
      }
      ```
    </div>
    </details>  

  - 설계 변경으로 `OrderServiceImpl`은 `FixDiscountPolicy`를 의존하지 않는다! 
  - 단지 `DiscountPolicy` 인터페이스만 의존한다. 
  - `OrderServiceImpl` 입장에서 생성자를 통해 어떤 구현 객체가 들어올지(주입될지)는 알 수 없다. 
  - `OrderServiceImpl`의 생성자를 통해서 어떤 구현 객체를 주입할지는 오직 외부(`AppConfig`)에서 결정한다.  
  - `OrderServiceImpl`은 이제부터 실행에만 집중하면 된다.  
  - `OrderServiceImpl`에는 `MemoryMemberRepository`, `FixDiscountPolicy` 객체의 의존관계가 주입된다.  
  
### AppConfig 등장
  
- 사용 클래스 - MemberApp
    <details>
    <summary>코드 보기</summary>
    <div markdown = "1">
      ```java  
      package hello.core;

      import hello.core.member.Grade;
      import hello.core.member.Member;
      import hello.core.member.MemberService;
      import hello.core.member.MemberServiceImpl;

      public class MemberApp {

          public static void main(String[] args) {
              // appconfig로 선택!
              AppConfig appConfig = new AppConfig();
              MemberService memberService = appConfig.memberService();

              Member member = new Member(1L, "memberA", Grade.VIP);
              memberService.join(member);

              Member findMember = memberService.findMember(1L);
              System.out.println("member = " + member.getName());
              System.out.println("findMember = " + findMember.getName());
          }
      }
      ```
    </div>
    </details>  
  
- 사용 클래스 - OrderApp
    <details>
    <summary>코드 보기</summary>
    <div markdown = "1">
      ```java  
      package hello.core;

      import hello.core.member.Grade;
      import hello.core.member.Member;
      import hello.core.member.MemberService;
      import hello.core.member.MemberServiceImpl;
      import hello.core.order.Order;
      import hello.core.order.OrderService;
      import hello.core.order.OrderServiceImpl;

      public class OrderApp {
        public static void main(String[] args) {

            //Appconfig로 선택!!
            AppConfig appConfig = new AppConfig();
            MemberService memberService = appConfig.memberService();
            OrderService orderService = appConfig.orderService();

            Long memberId = 1L;
            Member member = new Member(memberId,"memberA", Grade.VIP);
            memberService.join(member);

            Order order = orderService.createOrder(memberId, "itemA", 10000);

            System.out.println("order = " + order);
            System.out.println("order.calculatePrice() = " + order.calculatePrice());
        }
      }
      ```
    </div>
    </details>  
- 테스트 코드 오류 수정
    <details>
    <summary>코드 보기 - MemeberServiceTest</summary>
    <div markdown = "1">
      ```java  
      package hello.core.member;

      import hello.core.AppConfig;
      import org.assertj.core.api.Assertions;
      import org.junit.jupiter.api.BeforeEach;
      import org.junit.jupiter.api.Test;

      public class MemeberServiceTest {

          MemberService memberService;

          @BeforeEach
          public void beforeEach(){
              AppConfig appConfig = new AppConfig();
              memberService = appConfig.memberService();
          }

          @Test
          void join(){
              //given
              Member member = new Member(1L, "memberA", Grade.VIP);

              //when
              memberService.join(member);
              Member findMember = memberService.findMember(1L);

              //then
              Assertions.assertThat(member).isEqualTo(findMember);
              //똑같으면 성공

          }
      }

      ```
    </div>
    </details>  

    <details>
    <summary>코드 보기 - OrderServiceTest</summary>
    <div markdown = "1">
      ```java  
      package hello.core.order;

      import hello.core.AppConfig;
      import hello.core.member.Grade;
      import hello.core.member.Member;
      import hello.core.member.MemberService;
      import hello.core.member.MemberServiceImpl;

      import org.assertj.core.api.Assertions;
      import org.junit.jupiter.api.BeforeEach;
      import org.junit.jupiter.api.Test;

      public class OrderServiceTest {
          OrderService orderService;
          MemberService memberService;

          @BeforeEach
          void beforeEach(){
              AppConfig appConfig = new AppConfig();
              memberService = appConfig.memberService();
              orderService = appConfig.orderService();
          }

          @Test
          void createOrder(){
              Long memberId = 1L;
              Member member = new Member(memberId, "memberA", Grade.VIP);
              memberService.join(member);

              Order order = orderService.createOrder(memberId, "itemA", 10000);
              Assertions.assertThat(order.getDiscountPrice()).isEqualTo(1000);
          }
      }
      ```
    </div>
    </details>  
- 정리 
  - AppConfig를 통해서 관심사를 확실하게 분리하였다.  
    - 공연 기획자 = AppConfig
    - 배역 = Interface
    - 배우 = 구현체
  - AppConfig는 구체 클래스를 선택한다.
    - 배역에 맞는 담당 배우를 선택한다.
    - 애플리케이션이 어떻게 동작해야할 지 전체 구성을 책임진다.
  - 이제 각 배우들은 담당 배역을 소화하는 책임만 지면 된다.
  - `OrderServiceImpl`은 기능을 실행하는 책임만 지면 된다.  

끝-!😋