---
title: "[프로그래머스] [LV1] 제일 작은 수 제거하기"
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

# 📚 제일 작은 수 제거하기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12935>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/50-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int[] solution(int[] arr) {
        int[] answer = {};
        
        int minNum = 0;
        ArrayList<Integer> list = new ArrayList<Integer>();
        
        if(arr.length <= 1 ){
            answer = new int[1];
            answer[0] = -1;
        }
        else{
            minNum = arr[0];
            
            for(int i : arr)
                if(i < minNum) minNum = i;
            
            for(int i : arr)
                if(i != minNum) list.add(i);
            
            answer = new int[list.size()];
            
            for(int i = 0; i < list.size(); i++)
                answer[i] = list.get(i);
        }
        
        return answer;
    }
}
```  
  
끝-!
