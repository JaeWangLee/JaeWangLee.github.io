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
      ```java
        package hello.core.member;
          public enum Grade {
              BASIC,
              VIP 
          }  
      ```
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
  
- 회원 저장소 인터페이스(MemberRepository)
  - 저장소는 아직 미정이기 때문에 인터페이스를 만든다.
  - 회원의 `저장`과 `조회`를 구현.
    <details>
    <summary>코드 보기</summary>  
    <div markdown = "1">
      ```java  
        package hello.core.member;
        public interface MemberRepository {
            void save(Member member);
            Member findById(Long memberId);
        }
      ```
    </div>
    </details>
  
- 메모리 회원 저장소 구현체(MemoryMemberRepository)
  - DB가 미정이기 때문에 가장 단순한 메모리 회원 저장소를 구현한다.
  - 참고 : HashMap은 동시성 이슈가 발생할 수 있다. 이런 경우 `ConcurrentHashMap`을 사용하자.
    <details>
    <summary>코드 보기</summary>  
    <div markdown = "1">
      ```java  
      package hello.core.member;

      import java.util.HashMap;
      import java.util.Map;

      public class MemoryMemberRepository implements MemberRepository{

          private static Map<Long, Member> store = new HashMap<>();
          // 동시성 이슈가 있어 실무에서는 concurrent Hashmap 을 사용한다.

          @Override
          public void save(Member member) {
              store.put(member.getId(), member);
          }

          @Override
          public Member findById(Long memberId) {
              return store.get(memberId);
          }
      }
      ```
    </div>
    </details>
  
### 회원 서비스
  
- 회원 서비스 인터페이스(MemberService)
  - `가입`과 `조회`가 가능하도록 구현한다.
    <details>
    <summary>코드 보기</summary>  
    <div markdown = "1">
      ```java  
      package hello.core.member;
      public interface MemberService {
          void join(Member member);
          Member findMember(Long memberId);
      }
      ```
    </div>
    </details>
  
- 회원 서비스 구현체(MemberServiceImpl)
  - `memoryMemberRepository`를 구현체로한 회원 서비스 구현체를 구현한다.
    <details>
    <summary>코드 보기</summary>  
    <div markdown = "1">
      ```java  
      package hello.core.member;

      public class MemberServiceImpl implements MemberService{

          private final MemberRepository memberRepository = new MemoryMemberRepository();
          //구현체를 넣어준다.

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
  
## 2.5. 회원 도메인 실행과 테스트
  
### 회원 도메인 - 회원 가입 main
  
- 테스트를 위한 MemberApp
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
      MemberService memberService = new MemberServiceImpl();
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

- 그러나 애플리케이션 로직으로 테스트하는 것은 좋은 방법이 아니다.
- `JUnit 테스트`를 사용하자.
  
### 회원 도메인 - 회원 가입 테스트
  
- `JUnit`을 활용한 테스트 (MemberServiceTest)
  <details>
  <summary>코드 보기</summary>
  <div markdown = "1">
    ```java  
    package hello.core.member;

    import org.assertj.core.api.Assertions;
    import org.junit.jupiter.api.Test;

    public class MemeberServiceTest {

        MemberService memberService = new MemberServiceImpl();

        @Test
        void join(){
            //given
            Member member = new Member(1L, "memberA", Grade.VIP);

            //when
            memberService.join(member);
            Member findMember = memberService.findMember(1L);

            //then
            Assertions.assertThat(member).isEqualTo(findMember);
        }
    }
    ```
  </div>
  </details>
  
> **Assertions?**  
> Assertion은 불리언 식(expression)을 포함하고 있는 문장으로서, 프로그래머는 그 문장이 실행될 경우 불리언 식이 참이라고 단언할 수 있다.  
  
- **위 코드의 설계상 문제점은 무엇인가?**
  - 의존 관계가 인터페이스 뿐만 아니라 구현까지 모두 의존하는 문제점이 있음
    <details>
    <summary>코드 보기</summary>  
    <div markdown = "1">
      ```java  
      public class MemberServiceImpl implements MemberService{
          // 바로 이 부분 MemberRepository라는 인터페이스와 MemoryMemberRepository라는 구현체까지 의존!
          private final MemberRepository memberRepository = new MemoryMemberRepository();
      ```
    </div>
    </details>
  - 주문까지 만들고나서 문제점과 해결 방안을 설명할 예정
  
## 2.6. 주문과 할인 도메인 설계
  
- 주문 도메인 협력, 역할, 책임
  
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션2/5.png)  
  
  1. 주문 생성 : 클라이언트는 주문 서비스에 주문 생성을 요청한다.  
  2. 회원 조회 : 할인을 위해서는 회원 등급이 필요하다. 그래서 주문 서비스는 회원 저장소에서 회원을 조회한다.  
  3. 할인 적용 : 주문 서비스는 회원 등급에 따른 할인 여부를 할인 정책에 위임한다.  
  4. 주문 결과 반환 : 주문 서비스는 할인 결과를 포함한 주문 결과를 반환한다.  
  
- 주문 도메인 전체
  
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션2/6.png)  
  - **역할**과 **구현**을 분리하여 자유롭게 구현 객체를 조립할 수 있게 설계한다.  
  - 덕분에 회원 저장소는 물론, 할인 정책도 유연하게 변경할 수 있다.
  
- 주문 도메인 클래스 다이어그램
  
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션2/7.png)  
  - `impl`이라는 것은 인터페이스의 구현체가 1개일때 주로 붙여준다.
  
- 주문 도메인 객체 다이어그램1
  
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션2/8.png)  
  - 회원을 메모리에서 조회하고, 정액 할인 정책(고정 금액)을 지원해도 주문 서비스를 변경하지 않아도 된다.  
  - 역할들의 협력 관계를 그대로 재사용 할 수 있다.  
  
- 주문 도메인 객체 다이어그램2
  
  ![이미지](/assets/images/Spring/스프링_핵심_원리/섹션2/9.png)  
  - 회원을 메모리가 아닌 실제 DB에서 조회하고, 정률 할인 정책을 지원해도 주문 서비스를 변경하지 않아도 된다.  
  - 역할들의 협력 관계를 그대로 재사용 할 수 있다.  
  
## 2.7. 주문과 할인 도메인 개발
  
- 할인 정책 인터페이스(DiscountPolicy)
  - 회원 정보와 가격을 넘겨준다.
  - 회원 등급에 따라 할인 정책에 따른 할인된 금액을 반환해준다. 
  <details>
  <summary>코드 보기</summary>
  <div markdown = "1">
    ```java  
    package hello.core.discount;
    import hello.core.member.Member;
    public interface DiscountPolicy {

        /**
            @return 할인 대상 금액
        *
        */
        int discount(Member member, int price);
    }
    ```
  </div>
  </details>
  
- 할인 정책 구현체(FixDiscountPolicy)
  - 우선 정액 할인은 VIP인 경우에만 1000원 할인 적용한다.
  <details>
  <summary>코드 보기</summary>
  <div markdown = "1">
    ```java  
    package hello.core.discount;

    import hello.core.member.Grade;
    import hello.core.member.Member;

    public class FixDiscountPolicy implements DiscountPolicy{

        private int discountFixAmount = 1000; // 1000원 할인

        @Override
        public int discount(Member member, int price) {
            if(member.getGrade() == Grade.VIP)
                return discountFixAmount;
            else
                return 0;
        }
    }
    ```
  </div>
  </details>
  
- 주문 엔티티(Order)
  - 주문에 필요한 객체를 정의한다. 
  - 회원id, 상품명, 상품 가격, *할인된 가격*
  <details>
  <summary>코드 보기</summary>
  <div markdown = "1">
    ```java  
    package hello.core.order;

    public class Order {

        private Long memberId;
        private String itemName;
        private int itemPrice;
        private int discountPrice;

        public Order(Long memberId, String itemName, int itemPrice, int discountPrice) {
            this.memberId = memberId;
            this.itemName = itemName;
            this.itemPrice = itemPrice;
            this.discountPrice = discountPrice;
        }

        public int calculatePrice(){
            return itemPrice - discountPrice;
        }

        public Long getMemberId() {
            return memberId;
        }

        public void setMemberId(Long memberId) {
            this.memberId = memberId;
        }

        public String getItemName() {
            return itemName;
        }

        public void setItemName(String itemName) {
            this.itemName = itemName;
        }

        public int getItemPrice() {
            return itemPrice;
        }

        public void setItemPrice(int itemPrice) {
            this.itemPrice = itemPrice;
        }

        public int getDiscountPrice() {
            return discountPrice;
        }

        public void setDiscountPrice(int discountPrice) {
            this.discountPrice = discountPrice;
        }

        @Override // 출력을 편하게 할라구
        public String toString() {
            return "Order{" +
                    "memberId=" + memberId +
                    ", itemName='" + itemName + '\'' +
                    ", itemPrice=" + itemPrice +
                    ", discountPrice=" + discountPrice +
                    '}';
        }
    }

    ```
  </div>
  </details>
  
- 주문 서비스 인터페이스(OrderService)
  - 회원id, 상품명, 상품 가격을 넘겨주는 Order 생성하는 서비스 구현
  <details>
  <summary>코드 보기</summary>
  <div markdown = "1">
    ```java  
    package hello.core.order;
    public interface OrderService {
        Order createOrder(Long memberId, String itemName, int itemPrice);
    }
    ```
  </div>
  </details>
  
- 주문 서비스 구현체(OrderServiceImpl)
  - 주문 생성 요청이 오면, 회원정보를 조회하고, 할인 정책을 적용한 다음 주문 객체를 생성하여 반환한다.  
  - **메모리 회원 리포지토리와 고정 금액 할인 정책을 구현체로 한다.**
  <details>
  <summary>코드 보기</summary>
  <div markdown = "1">
    ```java  
    package hello.core.order;

    import hello.core.discount.DiscountPolicy;
    import hello.core.discount.FixDiscountPolicy;
    import hello.core.member.Member;
    import hello.core.member.MemberRepository;
    import hello.core.member.MemoryMemberRepository;

    public class OrderServiceImpl implements OrderService{

        private final MemberRepository memberRepository = new MemoryMemberRepository();
        private final DiscountPolicy discountPolicy = new FixDiscountPolicy();

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
  
## 2.8. 주문과 할인 도메인 실행과 테스트
  
끝-!😋