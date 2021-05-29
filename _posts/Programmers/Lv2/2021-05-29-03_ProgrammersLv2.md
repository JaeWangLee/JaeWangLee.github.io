---
title: "[프로그래머스] [LV2] 기능 개발"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-29 18:30:20
---

# 📚 가능 개발
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42586>  

>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob3/3-1.png)
![이미지](/assets/images/Programmers/Lv2/prob3/3-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        int[] answer = {};
        
        int step = 0;
        int count = 0;
        final int lastStep = progresses.length;
        List<Integer> list = new ArrayList<Integer>();
        
        while(step != lastStep){
            count = step;
            for(int i = step; i < lastStep; i++)
                progresses[i] += speeds[i];
            
            for(int i = step; i < lastStep; i++){
                if(progresses[i]>= 100){
                    step++;
                }
                else break;
            }
            
            if(step - count != 0) list.add(step - count);
        }
        
        answer = new int[list.size()];
        
        for(int i = 0; i < list.size(); i++)
            answer[i] = list.get(i);
        
        return answer;
    }
}
```  
  
끝-!
