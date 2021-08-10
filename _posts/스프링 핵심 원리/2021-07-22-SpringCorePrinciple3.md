---
title: "[스프링 핵심 원리 - 기본편] 섹션3. 스프링 핵심 원리 이해2 - 객체 지향 원리 적용"
excerpt: "스프링 입문 - 김영한 님의 강의 내용입니다."
toc: true
toc_sticky: true
categories:
  - Spring
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
  
### AppConfig 실행
  
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

## 3.4. AppConfig 리팩터링
  
현재 `AppConfig`는 중복이 존재하고, 역할에 따른 구현이 잘 보이지 않는다.  
  
- 기대하는 그림
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션3/8.png)  
  - 중복을 제거하고, 역할에 따른 구현이 보이도록 리팩터링!
    <details>
    <summary>코드 보기</summary>
    <div markdown = "1">
      ```java  
      package hello.core;

      import hello.core.discount.DiscountPolicy;
      import hello.core.discount.FixDiscountPolicy;
      import hello.core.member.MemberRepository;
      import hello.core.member.MemberService;
      import hello.core.member.MemberServiceImpl;
      import hello.core.member.MemoryMemberRepository;
      import hello.core.order.OrderService;
      import hello.core.order.OrderServiceImpl;

      public class AppConfig {

          public MemberService memberService(){
              return new MemberServiceImpl(memberRepository());
          }

          public MemberRepository memberRepository(){
              return new MemoryMemberRepository();
          }

          public DiscountPolicy discountPolicy() {
              return new FixDiscountPolicy();
          }

          public OrderService orderService(){
              return new OrderServiceImpl(
                  memberRepository(),
                  discountPolicy());
          }
      }
      ```
    </div>
    </details>  

    - `new MemoryMemberRepository()` 이 부분이 중복 제거되었다. 이제 `MemoryMemberRepository`를 다른 구현체로 변경할 때 한 부분만 변경하면 된다.  
    - `AppConfig`를 보면 역할과 구현 클래스가 한눈에 들어온다. 애플리케이션 전체 구성이 어떻게 되어있는지 빠르게 파악할 수 있다.  

## 3.5. 새로운 구조와 할인 정책 적용
  
- `AppConfig`의 등장으로 애플리케이션을 사용 영역과, 객체를 생성하고 구성하는 영역으로 분리하였다.  
- 처음으로 돌아가서 정액 할인 정책을 정률(%)할인 정책으로 변경해보자.  
- `FixDiscountPolicy` ➡️ `RateDiscountPolicy`  
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션3/9.png)  
    <details>
    <summary>코드 보기 - 할인 정책 변경 코드</summary>
    <div markdown = "1">
      ```java  
      package hello.core;

      import hello.core.discount.DiscountPolicy;
      import hello.core.discount.FixDiscountPolicy;
      import hello.core.discount.RateDiscountPolicy;
      import hello.core.member.MemberRepository;
      import hello.core.member.MemberService;
      import hello.core.member.MemberServiceImpl;
      import hello.core.member.MemoryMemberRepository;
      import hello.core.order.OrderService;
      import hello.core.order.OrderServiceImpl;

      public class AppConfig {

          public MemberService memberService(){
              return new MemberServiceImpl(memberRepository());
          }

          public MemberRepository memberRepository(){
              return new MemoryMemberRepository();
          }

          public DiscountPolicy discountPolicy() {
              //return new FixDiscountPolicy(); 변경!
              return new RateDiscountPolicy();
          }

          public OrderService orderService(){
              return new OrderServiceImpl(
                  memberRepository(),
                  discountPolicy());
          }
      }
      ```
    </div>
    </details>  

    - `AppConfig`에서 할인 정책을 `RateDiscountPolicy`로 변경하였다.
    - 즉, 이제는 클라이언트 코드를 포함한 사용 영역에서 어떤 코드도 변경할 필요가 없다. 
    - 애플리케이션의 구성 역할을 담당하는 `AppConfig`만 변경하면 된다는 것이다. 
  
## 3.6. 전체 흐름 정리
  
- 새로운 할인 정책 개발
  - 다형성 덕분에 새로운 정률 할인 정책 코드를 추가로 개발하는 것 자체는 아무 문제가 없음
  
- 새로운 할인 정책 적용과 문제점
  - 새로 개발한 정률 할인 정책을 적용하려고 하니 클라이언트 코드인 주문 서비스 구현체도 함께 변경해야함 
  - 주문 서비스 클라이언트가 인터페이스인 `DiscountPolicy` 뿐만 아니라, 구체 클래스인 `FixDiscountPolicy` 도 함께 의존 ➡️ **DIP 위반**
  
- 관심사의 분리 ⭐️
  - 애플리케이션을 하나의 공연으로 생각
  - 기존에는 클라이언트가 의존하는 서버 구현 객체를 직접 생성하고, 실행함
  - 비유를 하면 기존에는 남자 주인공 배우가 공연도 하고, 동시에 여자 주인공도 직접 초빙하는 다양한 책임을 가지고 있음
  - 공연을 구성하고, 담당 배우를 섭외하고, 지정하는 책임을 담당하는 별도의 <u>공연 기획자</u>가 나올 시점
  - 공연 기획자인 `AppConfig`가 등장
  - `AppConfig`는 애플리케이션의 전체 동작 방식을 구성(config)하기 위해, **구현 객체를 생성**하고, **연결**하는 책임
  - 이제부터 클라이언트 객체는 자신의 역할을 실행하는 것만 집중, 권한이 줄어듬(책임이 명확해짐)
  
- AppConfig 리팩터링
  - 구성 정보에서 역할과 구현을 명확하게 분리 
  - 역할이 잘 들어남
  - 중복 제거
  
- 새로운 구조와 할인 정책 적용
  - 정액 할인 정책 정률% 할인 정책으로 변경
  - `AppConfig`의 등장으로 애플리케이션이 크게 사용 영역과, 객체를 생성하고 구성(Configuration)하는 영역으로 분리
  - 할인 정책을 변경해도 `AppConfig`가 있는 구성 영역만 변경하면 됨, 사용 영역은 변경할 필요가 없음. 
  - 물론 클라이언트 코드인 주문 서비스 코드도 변경하지 않음
  
## 3.7. 좋은 객체 지향 설계의 5가지 원칙(SOLID) 적용
  
여기서는 5가지 중 3가지를 적용하였다. (SRP, DIP, OCP)  
  
- **SRP 단일 책임 원칙**
  **"하나의 클래스는 하나의 책임만 가져야 한다."**
  - 클라이언트 객체는 직접 구현 객체를 생성하고, 연결하고, 실행하는 다양한 책임을 가지고 있음 
  - SRP 단일 책임 원칙을 따르면서 관심사를 분리함
  - 구현 객체를 생성하고 연결하는 책임은 `AppConfig`가 담당
  - 클라이언트 객체는 실행하는 책임만 담당
  
- **DIP 의존관계 역전 원칙**
  프로그래머는 **“추상화에 의존해야지, 구체화에 의존하면 안된다.”**
  - 의존성 주입은 이 원칙을 따르는 방법 중 하나다.
  - 새로운 할인 정책을 개발하고, 적용하려고 하니 클라이언트 코드도 함께 변경해야 했다. 
  - 왜냐하면 기존 클라이언트 코드(`OrderServiceImpl`)는 DIP를 지키며 `DiscountPolicy` 추상화 인터페이스에 의존하는 것 같았지만, <u> `FixDiscountPolicy` 구체화 구현 클래스에도 함께 의존했다.</u>
  - 클라이언트 코드가 `DiscountPolicy` 추상화 인터페이스에만 의존하도록 코드를 변경했다.
  - 하지만 클라이언트 코드는 인터페이스만으로는 아무것도 실행할 수 없다.
  - `AppConfig`가 `FixDiscountPolicy` 객체 인스턴스를 클라이언트 코드 대신 생성해서 클라이언트 코드 에 의존관계를 주입했다. 
  - 이렇게 해서 DIP 원칙을 따르면서 문제도 해결했다.
  
- **OCP**
  **소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다**
  - 다형성 사용하고 클라이언트가 DIP를 지킴
  - 애플리케이션을 사용 영역과 구성 영역으로 나눔
  - `AppConfig`가 의존관계를 `FixDiscountPolicy` `RateDiscountPolicy` 로 변경해서 클라이언트 코드에 주입하므로 클라이언트 코드는 변경하지 않아도 됨
  - 소프트웨어 요소를 새롭게 확장해도 사용 역영의 변경은 닫혀 있다!
  
## 3.8. IoC, DI, 그리고 컨테이너
  
- 제어의 역전 IoC(Inversion of Control)
  - 기존 프로그램은 클라이언트 구현 객체가 스스로 필요한 서버 구현 객체를 생성하고, 연결하고, 실행했다. 
  - 한 마디로 구현 객체가 프로그램의 제어 흐름을 스스로 조종했다. 
  - 반면에 `AppConfig`가 등장한 이후에 구현 객체는 자신의 로직을 실행하는 역할만 담당한다.
  - 프로그램의 제어 흐름은 이제 `AppConfig`가 가져간다. 
  - 즉, 프로그램에 대한 제어 흐름에 대한 권한은 모두 `AppConfig`가 가지고 있다. 
  - 이렇듯 <u>프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것</u>을 **제어의 역전(IoC)**이라 한다.
  
- 프레임워크 vs 라이브러리
  - 프레임워크가 내가 작성한 코드를 제어하고, 대신 실행하면 그것은 프레임워크가 맞다. (JUnit)
  - 반면에 내가 작성한 코드가 직접 제어의 흐름을 담당한다면 그것은 프레임워크가 아니라 라이브러리다.
  
- 의존관계 주입 DI(Dependency Injection)
  - `OrderServiceImpl`은 `DiscountPolicy` 인터페이스에 의존한다. 실제 어떤 구현 객체가 사용될지는 모른다.
  - 의존관계는 <u>정적인 클래스 의존 관계와, 실행 시점에 결정되는 동적인 객체(인스턴스) 의존 관계</u> 둘을 분리 해서 생각해야 한다.
  
- 정적인 클래스 의존관계
  - 클래스가 사용하는 import 코드만 보고 의존관계를 쉽게 판단할 수 있다. 정적인 의존관계는 애플리케이션을 실행하지 않아도 분석할 수 있다. 
  - 클래스 다이어그램을 보자
    ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션3/10.png)  
    - `OrderServiceImpl`은 `MemberRepository`, `DiscountPolicy` 에 의존한다는 것을 알 수 있다.
    - 그런데 이러한 클래스 의존관계 만으로는 실제 어떤 객체가 `OrderServiceImpl` 에 주입 될지 알 수 없다.
  
- 동적인 객체 인스턴스 의존관계
  애플리케이션 실행 시점에 실제 생성된 객체 인스턴스의 참조가 연결된 의존 관계다.
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션3/11.png)  
  - 애플리케이션 **실행 시점(런타임)**에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결 되는 것을 **의존관계 주입**이라 한다.
  - 객체 인스턴스를 생성하고, 그 참조값을 전달해서 연결된다.
  - 의존관계 주입을 사용하면 클라이언트 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있다.
  - 의존관계 주입을 사용하면 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다.
  
- IoC 컨테이너, DI 컨테이너
  - AppConfig 처럼 객체를 생성하고 관리하면서 의존관계를 연결해 주는 것을 IoC 컨테이너 또는 **DI 컨테이너**라 한다.
  - 의존관계 주입에 초점을 맞추어 최근에는 주로 DI 컨테이너라 한다.
  - 또는 어샘블러, 오브젝트 팩토리 등으로 불리기도 한다.
  
## 3.9. 스프링으로 전환하기
  
- **`AppConfig`를 스프링 기반으로 변경**
  
    <details>
    <summary>코드 보기</summary>
    <div markdown = "1">
      ```java  
      package hello.core;

      import hello.core.discount.DiscountPolicy;
      import hello.core.discount.FixDiscountPolicy;
      import hello.core.discount.RateDiscountPolicy;
      import hello.core.member.MemberRepository;
      import hello.core.member.MemberService;
      import hello.core.member.MemberServiceImpl;
      import hello.core.member.MemoryMemberRepository;
      import hello.core.order.OrderService;
      import hello.core.order.OrderServiceImpl;
      import org.springframework.context.annotation.Bean;
      import org.springframework.context.annotation.Configuration;

      @Configuration
      public class AppConfig {

          @Bean
          public MemberService memberService(){
              return new MemberServiceImpl(memberRepository());
          }

          @Bean
          public MemberRepository memberRepository(){
              return new MemoryMemberRepository();
          }

          @Bean
          public DiscountPolicy discountPolicy() {
              //return new FixDiscountPolicy();
              return new RateDiscountPolicy();
          }

          @Bean
          public OrderService orderService(){
              return new OrderServiceImpl(
                  memberRepository(),
                  discountPolicy());
          }
      }
      ```
    </div>
    </details>  

  - `AppConfig`에 설정을 구성한다는 뜻의 `@Configuration`을 붙여준다.
  - 각 메서드에 `@Bean`을 붙여준다. 이렇게 하면 스프링 컨테이너에 스프링 빈으로 등록한다.
  
- **`MemberApp`에 스프링 컨테이너 적용**
  
    <details>
    <summary>코드 보기</summary>
    <div markdown = "1">
      ```java  
      package hello.core;

      import hello.core.member.Grade;
      import hello.core.member.Member;
      import hello.core.member.MemberService;
      import hello.core.member.MemberServiceImpl;
      import org.springframework.context.ApplicationContext;
      import org.springframework.context.annotation.AnnotationConfigApplicationContext;

      public class MemberApp {

          public static void main(String[] args) {
              //AppConfig appConfig = new AppConfig();
              //MemberService memberService = appConfig.memberService();

              ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
              MemberService memberService = applicationContext.getBean("memberService", MemberService.class);

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
    
- **`OrderApp`에 스프링 컨테이너 적용**
  
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
      import org.springframework.context.ApplicationContext;
      import org.springframework.context.annotation.AnnotationConfigApplicationContext;

      public class OrderApp {
        public static void main(String[] args) {

        //AppConfig appConfig = new AppConfig();
        //MemberService memberService = appConfig.memberService();
        //OrderService orderService = appConfig.orderService();

        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
        MemberService memberService = applicationContext.getBean("memberService", MemberService.class);
        OrderService orderService = applicationContext.getBean("orderService", OrderService.class);

        Long memberId = 1L;
        Member member = new Member(memberId,"memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "itemA", 20000);

        System.out.println("order = " + order);
        System.out.println("order.calculatePrice() = " + order.calculatePrice());
        }
      }
      ```
    </div>
    </details>  
  
> 두 코드를 실행하면 스프링 관련 로그가 몇 줄 실행되면서 기존과 동일한 결과가 출력된다.  
  
- **스프링 컨테이너**
  - `ApplicationContext`를 스프링 컨테이너라 한다.
  - 기존에는 개발자가 `AppConfig`를 사용해서 직접 객체를 생성하고 DI를 했지만, 이제부터는 스프링 컨테이너를 통해서 사용한다.
  - 스프링 컨테이너는 `@Configuration`이 붙은 `AppConfig`를 설정(구성)정보로 사용한다. 
  - 여기서 `@Bean`이라 적힌 메서드를 모두 호출해서 반환된 객체를 스프링 컨테이너에 등록한다. 이렇게 스프링 컨테이너에 등록된 객체를 스프링 빈이라 한다.
  - 스프링 빈은 `@Bean` 이 붙은 메서드의 명을 스프링 빈의 이름으로 사용한다. (`memberService`, `orderService`)
  - 이전에는 개발자가 필요한 객체를 `AppConfig` 를 사용해서 직접 조회했지만, 이제부터는 스프링 컨테이너를 통해서 필요한 스프링 빈(객체)를 찾아야 한다. 스프링 빈은 `applicationContext.getBean()` 메서드를 사용해서 찾을 수 있다.
  - 기존에는 개발자가 직접 자바코드로 모든 것을 했다면 이제부터는 스프링 컨테이너에 객체를 스프링 빈으로 등록하고, 스프링 컨테이너에서 스프링 빈을 찾아서 사용하도록 변경되었다.
  
코드가 약간 더 복잡해진 것 같은데, 스프링 컨테이너를 사용하면 어떤 장점이 있을까?🤔
  
끝-!😋