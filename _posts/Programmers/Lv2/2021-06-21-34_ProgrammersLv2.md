---
title: "[프로그래머스] [LV2] 올바른 괄호"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - stack
last_modified_at: 2021-06-21 22:09:20
---

# 📚 올바른 괄호
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12909#>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob34/34-1.png)

## 📝 내 풀이  
    
```java  
import java.util.*;

class Solution {
    boolean solution(String s) {
        boolean answer = false;
        Stack<Character> buf = new Stack<Character>();
        
        if(s.charAt(0) == ')') return false;
            
        for(int i = 0; i < s.length(); i++){
            if(s.charAt(i) == '(') buf.push('(');
            else{
                if(buf.isEmpty()) return false;
                else if(buf.peek() == '(') buf.pop();
                else buf.push(')');
            }
        } 
        
        if(buf.isEmpty()) answer = true;

        return answer;
    }
}
```
  
## 👊🏻 내 전략
  
1. stack의 기본 문법을 활용하면 충분히 풀 수 있다.
2. 중요한 점은 Peek는 exception 에러 발생할 수 있으니 주의하자.
  
끝-!
