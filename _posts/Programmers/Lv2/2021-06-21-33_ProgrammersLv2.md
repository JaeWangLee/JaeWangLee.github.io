---
title: "[프로그래머스] [LV2] 최솟값 만들기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - sort
last_modified_at: 2021-06-21 21:52:20
---

# 📚 최솟값 만들기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12941>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob33/33-1.png)
![이미지](/assets/images/Programmers/Lv2/prob33/33-2.png)

## 📝 내 풀이  
    
```java  
import java.util.*;

class Solution
{
    public int solution(int []A, int []B)
    {
        int answer = 0;
        
        Arrays.sort(A);
        Arrays.sort(B);
        
        for(int i = 0; i < A.length; i++)
            answer += A[A.length-1-i]*B[i];

        return answer;
    }
}
```
  
## 👊🏻 내 전략
  
1. Sort를 해서 A의 가장 큰 값 * B의 가장 큰 값의 형태로 진행되도록 했다.
2. Arrays.sort()의 내림차순(역순)은 int는 안된다. Integer가 됨..
  
끝-!
