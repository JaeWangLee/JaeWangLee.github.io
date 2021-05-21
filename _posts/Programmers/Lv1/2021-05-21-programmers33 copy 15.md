---
title: "[프로그래머스] [LV1] 나누어 떨어지는 숫자 배열"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 00:15:20
---

# 📚 나누어 떨어지는 숫자 배열
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12910>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/28-1.png)
![이미지](/assets/images/Programmers/Lv1/28-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int[] solution(int[] arr, int divisor) {
        ArrayList<Integer> list = new ArrayList<Integer>();
        
        for(int i = 0; i < arr.length; i++)
            if(arr[i] % divisor == 0) list.add(arr[i]);
        
        int[] answer = new int[list.size()];
        
        if(list.size() == 0) {
            int[] wrong = {-1};
            return wrong;
        }
        else{
            for(int i = 0; i < answer.length; i++)
                answer[i] = list.get(i);
            
            Arrays.sort(answer);
        }
        
        
        return answer;
    }
}
```  
  
끝-!
