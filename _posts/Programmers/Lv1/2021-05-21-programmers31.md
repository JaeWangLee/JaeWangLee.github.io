---
title: "[프로그래머스] [LV1] 수박수박수박수박수박수?"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 22:10:20
---

# 📚 수박수박수박수박수박수?
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12922>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/31-1.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public String solution(int n) {
        String answer = "";
        
        for(int i = 0; i < n; i++){
            if(i % 2 == 0) answer += "수";
            else answer += "박";
        }
        
        return answer;
    }
}
```  
  
끝-!
