---
title: "[프로그래머스] [LV1] 내적"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-03 23:05:20
---

# 📚 내적
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/70128>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/12-1.png)
![이미지](/assets/images/Programmers/Lv1/12-2.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public int solution(int[] a, int[] b) {
        int answer = 0;
        
        for(int i = 0 ; i < a.length; i++){
            answer += a[i]*b[i];
        }
        
        return answer;
    }
}
```
  
Super easy...
  
끝-!