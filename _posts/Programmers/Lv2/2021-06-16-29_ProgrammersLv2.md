---
title: "[프로그래머스] [LV2] 피보나치 수"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-16 22:08:20
---

# 📚 피보나치 수
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12945>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob29/29-1.png)
![이미지](/assets/images/Programmers/Lv2/prob29/29-2.png)
  
## 📝 내 풀이  
    
```java  
class Solution {
    public int solution(int n) {
        int[] answer = new int[n+1];
        
        answer[0] = 0;
        answer[1] = 1;
        for(int i = 2; i < n+1; i++)
            answer[i] = (answer[i-1] + answer[i-2])%1234567;

        return answer[n];
    }
}
```
  
## 👊🏻 내 전략
  
처음에는 재귀를 사용해서 풀이를 했으나, 7번부터 시간 초과가 났다.  
알아보니 *"피보나치 수가 커질수록 그 크기는 기하급수적으로 커지고, 44번째 피보나치에서는 이미 int 범위를 넘어선다."* 는 것이다.  
그래서 매번 계산 결과에 대해 %1234567을 진행하도록 문제가 유도 되있었다.  
> 모듈러 연산의 법칙  
> 숫자 A, B, C가 있다고 하면 (A + B) % C의 값은 ( ( A % C ) + ( B % C) ) % C와 같다  

그래서 매번 값을 기억할 수 있도록 배열의 형태로 각 단계의 결과값을 저장하되,  
1234567로 나눈 나머지 값을 저장하게 하여 풀이하였다.  
  
끝-!
