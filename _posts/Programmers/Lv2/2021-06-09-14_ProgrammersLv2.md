---
title: "[프로그래머스] [LV2] 위장"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - Hash
last_modified_at: 2021-06-09 22:00:20
---

# 📚 위장
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42578?language=java#>  
  
>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob14/14-1.png)
![이미지](/assets/images/Programmers/Lv2/prob14/14-2.png)
![이미지](/assets/images/Programmers/Lv2/prob14/14-3.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(String[][] clothes) {
        int answer = 1;
        
        HashMap<String, Integer> map = new HashMap<String, Integer>();
        
        for(int i = 0; i < clothes.length; i++){
            if(!map.containsKey(clothes[i][1])) map.put(clothes[i][1], 1);
            else map.put(clothes[i][1], map.get(clothes[i][1])+1);
        }
        
        for(String key : map.keySet())
            answer *= (map.get(key)+1);
        
        return answer-1;
    }
}
```  
   
## 👊🏻 전략  
  
1. 조합의 수를 생각하자
   ex) 4종류일 경우 = (종류1+1)x(종류2+1)x(종류3+1)x(종류4+1) - 1  
2. 조합을 Hash의 key-value로 관리하자  
  
끝-!
