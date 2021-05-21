---
title: "[프로그래머스] [LV1] 시저 암호"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 22:21:20
---

# 📚 시저 암호
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12926>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/32-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public String solution(String s, int n) {
        String answer = "";
        
        for(int i = 0; i < s.length(); i++){
            if(Character.isUpperCase(s.charAt(i))){
                if(s.charAt(i) + n > 'Z') answer += (char)(s.charAt(i) + n - 1 - 'Z' + 'A');
                else answer += (char)(s.charAt(i) + n);
            }
            else if(Character.isLowerCase(s.charAt(i))){
                if(s.charAt(i) + n > 'z') answer += (char)(s.charAt(i) + n - 1 - 'z' + 'a');
                else answer += (char)(s.charAt(i) + n);
            }
            else answer += " ";
        }
        
        return answer;
    }
}
```  
  
끝-!
