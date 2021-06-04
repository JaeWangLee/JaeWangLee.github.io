---
title: "[프로그래머스] [LV2] 멀쩡한 사각형"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-04 23:17:20
---

# 📚 멀쩡한 사각형
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/62048>  

>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob5/5-1.png)
![이미지](/assets/images/Programmers/Lv2/prob5/5-2.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public long solution(int w, int h) {
        long answer = 1;
        long gcd = w > h ? h : w;
        
        while(!(w%gcd == 0 && h%gcd==0))
            gcd--;
        
        answer = ((long)w*h) - (w+h - 1*gcd) ;
        return answer;
    }
}
```  
  
이 문제를 풀기 위해서는  
1. 주어진 예제에서 패턴 찾기  
2. 자료형 주의할 것
두가지를 유념하고 풀어야 할 거 같다.  
  
![이미지](/assets/images/Programmers/Lv2/prob5/5-3.jpg)
  
끝-!
