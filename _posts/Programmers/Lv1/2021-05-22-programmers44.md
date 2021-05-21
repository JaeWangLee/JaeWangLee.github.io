---
title: "[프로그래머스] [LV1] 최대공약수와 최소공배수"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-22 00:04:20
---

# 📚 최대공약수와 최소공배수
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12940>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/44-1.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public int[] solution(int n, int m) {
        int[] answer = new int[2];
        int min = n < m ? n : m;
        
        for(int i = 1; i <= min; i++)
            if(n % i == 0 && m % i == 0) answer[0] = i;
        
        answer[1] = n*m / answer[0];
        
        return answer;
    }
}
```  
  
끝-!
