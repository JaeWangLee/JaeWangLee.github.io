---
title: "[프로그래머스] [LV1] 핸드폰 번호 가리기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 00:15:20
---

# 📚 핸드폰 번호 가리기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12948?language=java>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/46-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public String solution(String phone_number) {
        String answer = "";
        
        for(int i = 0; i < phone_number.length()-4; i++)
            answer += "*";
        
        answer += phone_number.substring(phone_number.length() - 4);
        return answer;
    }
}
```  
  
끝-!
