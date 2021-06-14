---
title: "[프로그래머스] [LV2] N개의 최소 공배수"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-14 21:13:20
---

# 📚 N개의 최소 공배수
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12953>  
  
>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob24/24-1.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public int solution(int[] arr) {
        int count = 0;
        int MaxNum = arr[arr.length-1];
        int answer = 0;
        
        while(count != arr.length){
            answer += MaxNum;
            count = 0;
            for(int i : arr){
                if(answer%i != 0) break; 
                count++;
            }
       }
        
        return answer;
    }
}
```
   
끝-!
