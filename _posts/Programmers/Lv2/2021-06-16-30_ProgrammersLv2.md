---
title: "[프로그래머스] [LV2] 더 맵게"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - Priority Queue
last_modified_at: 2021-06-16 22:48:20
---

# 📚 더 맵게
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42626?language=java>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob30/30-1.png)
![이미지](/assets/images/Programmers/Lv2/prob30/30-2.png)
  
## 📝 내 풀이  
    
```java  
import java.util.*;

class Solution {
    public int solution(int[] scoville, int K) {
        int answer = 0;
        PriorityQueue<Integer> pq = new PriorityQueue<>();
           
        for(int i : scoville)
            if(i<K) pq.add(i);
        
        while(pq.peek() < K){
            if(pq.size()==1) return -1;
            
            pq.add(pq.poll() + pq.poll()*2);
            answer++;
        }
    
        return answer;
    }
}
```
  
## 👊🏻 내 전략
  
1. 효율을 위해 scoville 배열에서 K를 초과하는 수는 배제시킨다.
2. 처음에는 ArrayList를 사용하였으나, 효율성 통과를 하지 못한다.
3. 우선 순위 Queue를 사용하자. 
   - 제일 우선 순위가 되는 1개만 고려하는 queue이며,
   - 나중에 자세히 다루겠다. 
  
끝-!
