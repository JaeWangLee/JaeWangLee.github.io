---
title: "[프로그래머스] [LV1] 자연수 뒤집어 배열로 만들기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 22:53:20
---

# 📚 자연수 뒤집어 배열로 만들기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12932>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/33-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int[] solution(long n) {
        ArrayList<Long> list = new ArrayList<Long>();
        
        while(n != 0){
            list.add(n % 10);
            n /= 10;
        }
        
        int[] answer = new int[list.size()];
        
        for(int i = 0; i < list.size(); i++)
            answer[i] = list.get(i).intValue();
        
        return answer;
    }
}
```  
  
끝-!
