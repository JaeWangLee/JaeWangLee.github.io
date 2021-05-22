---
title: "[프로그래머스] [LV1] 이상한 문자 만들기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-22 22:20:20
---

# 📚 이상한 문자 만들기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12930>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/51-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public String solution(String s) {
        String answer = "";
        int count = 0;
        
        for(int i = 0; i < s.length(); i++){
            if(s.charAt(i) == ' '){
                answer += ' ';
                count = 0;
                continue;
            }
            
            answer += count % 2 == 0 ? Character.toUpperCase(s.charAt(i)) : Character.toLowerCase(s.charAt(i));
            
            count++;
        }
        
        return answer;
    }
}
```  
  
끝-!
