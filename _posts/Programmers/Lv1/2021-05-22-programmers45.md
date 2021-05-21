---
title: "[프로그래머스] [LV1] 평균 구하기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-22 00:15:20
---

# 📚 평균 구하기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12944>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/45-1.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public double solution(int[] arr) {
        double answer = 0;
        
        for(int i : arr)
            answer += i;
        
        answer /= arr.length;
        
        return answer;
    }
}
```  
  
끝-!
