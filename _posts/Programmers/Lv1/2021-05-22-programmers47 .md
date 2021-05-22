---
title: "[프로그래머스] [LV1] 짝수와 홀수"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-22 22:15:20
---

# 📚 짝수와 홀수
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12937>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/47-1.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public String solution(int num) {
        String answer = "";
        return (answer = (num%2 == 0) ? "Even" : "Odd" );
    }
}
```  
  
끝-!
