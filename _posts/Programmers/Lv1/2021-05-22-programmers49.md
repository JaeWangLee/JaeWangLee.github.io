---
title: "[프로그래머스] [LV1] 소수 찾기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-22 22:15:20
---

# 📚 소수 찾기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12921>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/49-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(int n) {
        int answer = 0;
        int[] list = new int[n+1];
        
        Arrays.fill(list, 1);
        
        list[0] = 0;
        list[1] = 0;
        
        for(int i = 2; i <= n; i++){
            if(list[i] == 0) continue;
            
            for(int j = 2; i * j <= n; j++)
                if(list[i*j] == 1) list[i*j] = 0;
        }
        
        for(int i : list)
            answer += i;
        
        return answer;
    }
}
```  
  
끝-!
