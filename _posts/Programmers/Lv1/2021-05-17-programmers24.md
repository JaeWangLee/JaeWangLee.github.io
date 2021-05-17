---
title: "[프로그래머스] [LV1] 예산"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-17 20:24:20
---

# 📚 예산
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12982?language=java>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/24-1.png)
![이미지](/assets/images/Programmers/Lv1/24-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(int[] d, int budget) {
        int answer = 0;
        int sum = 0;
        
        Arrays.sort(d);
        
        for(int i = 0; i < d.length; i++){
            sum += d[i];
            if(sum > budget) break;
            answer++;
        }
        
        return answer;
    }
}
```  
  
끝-!
