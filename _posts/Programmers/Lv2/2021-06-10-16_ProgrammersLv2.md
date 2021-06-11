---
title: "[프로그래머스] [LV2] 다리를 지나는 트럭"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - Queue
  - Stack
last_modified_at: 2021-06-10 22:06:20
---

# 📚 다리를 지나는 트럭
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42583>  
  
>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob16/16-1.png)
![이미지](/assets/images/Programmers/Lv2/prob16/16-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(int bridge_length, int weight, int[] truck_weights) {
        int answer = 0;
        
        int bridge_weight = 0;
        
        ArrayList<Integer> list = new ArrayList<Integer>();
        Queue<Integer> wq = new LinkedList<Integer>();
        Queue<Integer> iq = new LinkedList<Integer>();
        
        for(int i : truck_weights)
            wq.offer(i);
        
        for(int i = 0; i < bridge_length; i++)
            iq.offer(0);
        
        while(list.size()!=truck_weights.length){
            answer++;     
            
            if(iq.peek()!=0) {
                bridge_weight -= iq.peek();
                list.add(iq.poll());
            }
            else iq.poll();
            
            if(!wq.isEmpty() && bridge_weight + wq.peek() <= weight){ // 건널 수 있는 무게면
                bridge_weight += wq.peek();
                iq.offer(wq.poll());
            }
            else
                iq.offer(0);  

        }
        
        return answer;
    }
}
```  
   
## 👊🏻 전략  
  
1. Queue를 활용하자  
   - 다리 위에 있는 트럭
   - 대기 중인 트럭  
2. 다리를 지나는 weight 관리를 하자
  
끝-!
