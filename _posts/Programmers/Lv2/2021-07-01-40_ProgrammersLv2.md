---
title: "[프로그래머스] [LV2] 괄호 회전하기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - stack
  - queue
last_modified_at: 2021-07-01 11:50:20
---

# 📚 괄호 회전하기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/76502#>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob40/40-1.png)
![이미지](/assets/images/Programmers/Lv2/prob40/40-2.png)
![이미지](/assets/images/Programmers/Lv2/prob40/40-3.png)

## 📝 내 풀이  
    
```java  
import java.util.*;

class Solution {
    public int solution(String s) {
        int answer = 0;
        
        Queue <Character> q = new LinkedList<Character>();
        Stack <Character> st = new Stack<Character>();
        char c = ' ';
        
        for(int i = 0; i < s.length(); i++)
            q.add(s.charAt(i));
        
        for(int i = 0; i < s.length(); i++){
            for(int j = 0; j < s.length(); j++){
                if(st.isEmpty() || (q.peek() - st.peek() <= 0 || q.peek() - st.peek() >= 3)) st.push(q.peek());
                else {
                    st.pop();
                    while(st.size()>1){
                        c = st.pop();
                        if(c - st.peek() > 0 && c - st.peek() < 3 ) st.pop();
                        else{
                            st.push(c);
                            break;
                        }
                    }
                }
                
                q.add(q.poll());
            }
            
            if(st.isEmpty()) answer++;
            else st.clear();
            
            c = ' ';
            q.add(q.poll());
        }
    
        return answer;
    }
}
``` 
   
## 👊🏻 내 전략
  
1. 각 괄호가 아구(?)가 맞는지 확인하기 위한 로직을 짜는 게 핵심
2. Queue와 Stack을 이용해서 넣고 빼고를 쉽게 할 수 있게 하였다.
3. 아구가 맞는지는 아스키 코드를 참고하였는데, 
   - 각 괄호의 짝은 아스키 코드 값이 1 혹은 2만큼 차이가 났다.
   - 즉 ]이 나왔을때 앞에꺼가 [이면 빼기를 하면 0보다 크고 3보다 작은 차이가 나는지 체크했다.

   
  
끝-!
