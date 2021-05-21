---
title: "[프로그래머스] [LV1] 정수 제곱근 판별"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 23:10:20
---

# 📚 정수 제곱근 판별
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12934>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/34-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public long solution(long n) {
        long answer = 0;
        answer = n ==((long)Math.sqrt(n))*((long)Math.sqrt(n)) ? (long)Math.pow(Math.sqrt(n)+1,2) : -1;
        return answer;
    }
}
```  
  
끝-!
