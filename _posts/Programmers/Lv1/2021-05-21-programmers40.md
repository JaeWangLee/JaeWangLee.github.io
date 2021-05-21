---
title: "[프로그래머스] [LV1] 정수 내림차순으로 배치하기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 23:46:20
---

# 📚 정수 내림차순으로 배치하기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12933>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/40-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public long solution(long n) {
        long answer = 0;
        ArrayList<Long> list = new ArrayList<Long>();
        
        while(n!=0){
            list.add(n%10);
            n /= 10;
        }
        
        Collections.sort(list);
        Collections.reverse(list);
        
        for(long i : list)
            answer = answer * 10 + i;
        
        return answer;
    }
}
```  
  
끝-!
