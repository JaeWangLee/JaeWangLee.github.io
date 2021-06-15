---
title: "[프로그래머스] [LV2] 카펫"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-15 21:08:20
---

# 📚 카펫
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42842>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob27/27-1.png)
![이미지](/assets/images/Programmers/Lv2/prob27/27-2.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public int[] solution(int brown, int yellow) {
        int[] answer = new int[2];
        int sum = brown + yellow;
        
        for(int i = 1; i < sum; i++){
            if(sum%i!=0) continue;
            answer[1] = i;
            answer[0] = sum/i;
            if(brown == (answer[0]+answer[1])*2 - 4) break;
        }
        
        return answer;
    }
}
```
  
## 👊🏻 내 전략
  
1. brown과 yellow의 합은 answer[0]*answer[1]이다.
2. brown은 answer[0] + answer[1])*2 - 4이다.
   
끝-!
