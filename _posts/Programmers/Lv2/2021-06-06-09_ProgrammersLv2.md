---
title: "[프로그래머스] [LV2] 124 나라의 숫자"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-06 13:46:20
---

# 📚 124 나라의 숫자
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12899>  

>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob9/9-1.png)
![이미지](/assets/images/Programmers/Lv2/prob9/9-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public String solution(int n) {
        String answer = "";
        
        int reminder = 0; // 나머지
        Stack<Integer> stack = new Stack<Integer>();
        
        while(n!=0){
            reminder = n%3;
            n /= 3;
            
            if(reminder == 0){
                stack.push(4);
                n--;
            }
            else
                stack.push(reminder);
        }            
        
        while(!stack.empty())
            answer += stack.pop();
            
        return answer;
    }
}
```  
  
## 👊🏻 전략  
  
1. 1,3,9... 3진법으로 풀자.
2. 단 나머지가 0인 경우 /몫 - 1/ 나머지 + 4를 해준다.  
3. ArrrayList로 풀었더니 시간 초과가 난다. 
4. Stack으로 풀자.  
  
자료 구조는 귀국하면 얼른 배워야겠다. 😂  
  
끝-!
