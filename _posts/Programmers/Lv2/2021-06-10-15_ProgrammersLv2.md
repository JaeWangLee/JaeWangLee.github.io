---
title: "[프로그래머스] [LV2] H-Index"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - Array
last_modified_at: 2021-06-10 21:00:20
---

# 📚 H-Index
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42747#>  
  
>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob15/15-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(int[] citations) {
        int answer = 0;
        Integer arr[] = new Integer[citations.length];
        
        for(int i = 0; i < citations.length; i++) 
			arr[i] = citations[i];
        Arrays.sort(arr, Collections.reverseOrder());
        
        for(int i = arr[0]; i>=0; i--){
            answer = 0;
            for(int j = 0; j < arr.length; j++){
                if(arr[j] >= i) answer++;
                else break;
            }
            if(answer >= i) return i;
        }
        
        return answer;
    }
}
```  
   
## 📝 다른 풀이
  
```java
import java.util.*;

class Solution {
    public int solution(int[] citations) {
        Arrays.sort(citations);

        int max = 0;
        for(int i = citations.length-1; i > -1; i--){
            int min = (int)Math.min(citations[i], citations.length - i);
            if(max < min) max = min;
        }

        return max;
    }
}
```
  
끝-!
