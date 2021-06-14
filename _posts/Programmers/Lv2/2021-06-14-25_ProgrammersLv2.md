---
title: "[프로그래머스] [LV2] 숫자의 표현"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-14 21:28:20
---

# 📚 숫자의 표현
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12924>  
  
>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob25/25-1.png)
![이미지](/assets/images/Programmers/Lv2/prob25/25-2.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public int solution(int n) {
        int answer = 1;
        int sum = 0;
            
        for(int i = 1; i < n; i++){
            sum = i;
            for(int j = i+1; j <n; j++){
                sum += j;
                if(sum == n) {answer++; break;}
                else if(sum > n) break;
            }
        }
        
        return answer;
    }
}
```
   
끝-!
