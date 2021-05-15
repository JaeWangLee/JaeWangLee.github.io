---
title: "[프로그래머스] [LV1] 약수의 갯수와 덧셈"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-15 19:10:20
---

# 📚 약수의 갯수와 덧셈
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12950?language=java>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/17-1.png)
![이미지](/assets/images/Programmers/Lv1/17-2.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public int solution(int left, int right) {
        int answer = 0;
        
        for(int i = left; i <= right; i++)
            answer += getDivisor(i);
        
        return answer;
    }
    
    public int getDivisor(int n){
        int count = 0; 
        
        for(int i = 1; i <= n; i++)
            if(n%i == 0) count++;            
            
        n = count % 2 == 0 ? n : (-1*n);
        
        return n;
    }
}
```
  
  
끝-!