---
title: "[프로그래머스] [LV1] 가운데 글자 가져오기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-03 21:15:20
---

# 📚 가운데 글자 가져오기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12903>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/11-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public String solution(String s) {
        String answer = "";
        
        answer = s.length()%2==0 ? s.substring(s.length()/2-1, s.length()/2+1):s.substring(s.length()/2, s.length()/2+1);
        
        return answer;
    }
}
```
  
## ✔️ String의 길이  
String의 길이는 `.length()`  
배열의 길이는 `.length`  
Collection의 길이(크기)는 `.size()`  
  
끝-!