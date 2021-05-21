---
title: "[프로그래머스] [LV1] 약수의 합"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 23:39:20
---

# 📚 약수의 합
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12928>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/38-1.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public int solution(int n) {
        int answer = 0;
        
        for(int i = 1; i <= n; i++){
            if(n % i == 0) answer += i;
        }
        
        return answer;
    }
}
```  
  
끝-!
