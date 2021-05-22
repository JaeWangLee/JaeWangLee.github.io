---
title: "[프로그래머스] [LV1] x만큼 간격이 있는 n개의 숫자"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 00:15:20
---

# 📚 x만큼 간격이 있는 n개의 숫자
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12954?language=java>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/52-1.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public long[] solution(int x, int n) {
        long[] answer = new long[n];
        
        for(int i = 0; i < n; i++)
            answer[i] = x * (long)(i+1);
        
        return answer;
    }
}
```  
  
끝-!
