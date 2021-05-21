---
title: "[프로그래머스] [LV1] 콜라츠 추측"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 23:55:20
---

# 📚 콜라츠 추측
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12943>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/41-1.png)
![이미지](/assets/images/Programmers/Lv1/41-2.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public int solution(int num) {
        int answer = 0;
        long n = (long)num;
        
        while(n != 1 && answer != 500){
            if(n % 2 ==0)
                n /= 2;
            else
                n = n * 3 + 1;
            answer++;
        }
        
        answer = answer == 500 ? -1 : answer;
        
        return answer;
    }
}
```  
  
끝-!
