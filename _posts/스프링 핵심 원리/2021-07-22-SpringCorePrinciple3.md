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
  
새로운 할인 정책을 애플리케이션에 적용해보자.  
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
  
## 3.3. 관심사의 분리

끝-!😋