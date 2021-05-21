---
title: "[프로그래머스] [LV1] 문자열을 정수로 바꾸기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 23:38:20
---

# 📚 문자열을 정수로 바꾸기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12925>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/37-1.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public int solution(String s) {
        int answer = 0;
        int sign = 1;
        
        if(s.charAt(0) == '+' || s.charAt(0) == '-'){
            sign = s.charAt(0) == '+' ? 1 : -1;
        }
        else
            answer = s.charAt(0) - '0';
        
        for(int i = 1; i < s.length(); i++)
            answer = answer * 10 + s.charAt(i) - '0';
        
        return (sign*answer);
    }
}
```  
  
끝-!
