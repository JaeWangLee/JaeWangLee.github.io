---
title: "[프로그래머스] [LV1] 하샤드 수"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 23:55:20
---

# 📚 하샤드 수
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12947>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/42-1.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public boolean solution(int x) {
        boolean answer = true;
        int temp = x, sum = 0;
        
        while(temp != 0){
            sum += temp % 10;
            temp /= 10;
        }
        
        answer = x%sum == 0 ? true : false;
        
        return answer;
    }
}
```  
  
끝-!
