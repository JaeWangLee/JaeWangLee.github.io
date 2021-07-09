---
title: "[프로그래머스] [LV2] 짝지어 제거하기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-24 22:20:20
---
  
# 📚 짝지어 제거하기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12973>  

>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob1/1-1.png)
![이미지](/assets/images/Programmers/Lv2/prob1/1-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution
{
    public int solution(String s)
    {
        int answer = 0;
        Stack <Character> stk = new Stack<Character>();
        
        stk.push(s.charAt(0));
        
        for(int i = 1; i < s.length(); i++){
            if(stk.empty()) stk.push(s.charAt(i));
            else{
                if(stk.peek() == s.charAt(i)) stk.pop();
                else stk.push(s.charAt(i));        
            }
        }
        
        answer = stk.empty() ? 1 : 0;
        
        return answer;
    }
}
```  
  
끝-!
