---
title: "[프로그래머스] [LV1] 문자열 내림차순으로 배치하기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 21:45:20
---

# 📚 문자열 내림차순으로 배치하기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12917>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/29-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public String solution(String s) {
        String answer = "";
        ArrayList <Character> upperS = new ArrayList<Character>();
        ArrayList <Character> lowerS = new ArrayList<Character>();
        
        for(int i = 0; i < s.length(); i++){
            if(Character.isUpperCase(s.charAt(i))) upperS.add(s.charAt(i));
            else lowerS.add(s.charAt(i));
        }
        
        Collections.sort(upperS);
        Collections.reverse(upperS);
        
        Collections.sort(lowerS);
        Collections.reverse(lowerS);
        
        for(char i : lowerS)
            answer += i;
        for(char i : upperS)
            answer += i;
        
        return answer;
    }
}
```  
  
끝-!
