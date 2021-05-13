---
title: "[프로그래머스] [LV1] 음양 더하기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-03 15:38:20
---

# 📚 음양 더하기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/76501>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/9-1.png)

![이미지](/assets/images/Programmers/Lv1/9-2.png)

  
## 📝 내 풀이  
  
```java
class Solution {
    public int solution(int[] absolutes, boolean[] signs) {
        int answer = 0;
        
        for(int i = 0; i < absolutes.length; i++){
            answer += (signs[i] ? absolutes[i] : -1*absolutes[i]);
        }
        
        return answer;
    }
}
```
너무 쉬웠다..

끝-!