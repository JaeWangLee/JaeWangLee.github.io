---
title: "[프로그래머스] [LV2] 괄호 변환"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - queue
  - recursion
last_modified_at: 2021-07-07 21:30:20
---

# 📚 괄호 변환
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/60058#>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob46/46-1.png)
![이미지](/assets/images/Programmers/Lv2/prob46/46-2.png)
![이미지](/assets/images/Programmers/Lv2/prob46/46-3.png)

## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {    
    public String solution(String p) {
        String answer = "";
        Queue<Character> v = new LinkedList<Character>();

        for(int i = 0; i < p.length(); i++)
            v.offer(p.charAt(i));
        
        answer = transFormation(v);   
        
        return answer;
    }
    
    public String transFormation(Queue<Character> q){
        
        if(q.isEmpty()) return "";
        Queue<Character> u = new LinkedList<Character>();
        
        int chk = 0;
        String ans = "";
        boolean isCorrect = true;
            
        while(true){
            // 올바른 괄호 체크하기 위함 - 로직 매우 간단
            if(q.peek() == '(') chk++;
            else chk--;
            
            u.offer(q.poll());
            
            if(chk == 0) break;
            else if(chk < 0) isCorrect = false;
        }
                
        if(isCorrect){
            while(!u.isEmpty())
                ans += u.poll();
            return ans + transFormation(q);
        }
       
        ans += "(" + transFormation(q) + ")";
        u.remove();
        while(u.size()!=1){
            if(u.peek() == '(') ans += ")";
            else if(u.peek() == ')') ans += "(";
            u.remove();
        }
        
        return ans;
    }
}
``` 
  
## 👊🏻 내 전략 
  
1. 재귀를 쓰라고 문제에 주어졌으니 재귀를 쓰자.
   - 따로 함수를 구현하자
2. u, v 는 넣다 뺐다를 자주해야하니 queue를 사용하자.
  
끝-!
