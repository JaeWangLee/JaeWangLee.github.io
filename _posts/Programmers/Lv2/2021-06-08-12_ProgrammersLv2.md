---
title: "[프로그래머스] [LV2] 주식가격"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-08 22:00:20
---

# 📚 주식가격
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42584?language=java>  
  
>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob12/12-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int[] solution(int[] prices) {

        int[] answer = new int[prices.length];
        
        for(int i = 0; i < prices.length; i++){
            for(int j = i+1; j < prices.length; j++){
                answer[i]++;
                if(prices[i] > prices[j])
                    break;
            }
        }
        
        return answer;
    }
}
```  

## 👊🏻 전략  
  
이중 for문으로 현재 가격보다 떨어지는 지점을 찾아 break~
  
끝-!
