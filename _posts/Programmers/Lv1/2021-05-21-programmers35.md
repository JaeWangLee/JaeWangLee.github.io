---
title: "[프로그래머스] [LV1] 문자열 내 p와 y의 개수"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 23:30:20
---

# 📚 문자열 내 p와 y의 개수
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12916>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/35-1.png)
![이미지](/assets/images/Programmers/Lv1/35-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    boolean solution(String s) {
        boolean answer = true;
        int count=0;
        
        s = s.toLowerCase();
        
        for(int i = 0; i < s.length(); i++){
            if(s.charAt(i) == 'p') count++;
            else if(s.charAt(i)=='y') count--;
        }
        
        if(count != 0) answer = false;
        
        return answer;
    }
}
```  
  
끝-!
