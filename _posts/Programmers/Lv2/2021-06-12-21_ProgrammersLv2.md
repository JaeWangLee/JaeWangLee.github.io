---
title: "[프로그래머스] [LV2] 다음 큰 숫자"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-12 20:35:20
---

# 📚 다음 큰 숫자
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12911>  
  
>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob21/21-1.png)
![이미지](/assets/images/Programmers/Lv2/prob21/21-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(int n) {
        int answer = n + 1;
    int cnt1 = 0, cnt2 = 0;

    //1. n을 이진수로 했을때 1의 갯수 세기
    while(n!=0)
    {
        if(n%2 == 1) cnt1++;
        n /= 2;
    }
        
    //2. n보다 큰 수 while돌리면서 1의 갯수 동일하면 break;
    while(true)
    {
        cnt2 = 0;
        n = answer;
        
         while(n!=0)
        {
            if(n%2 == 1) cnt2++;
            n /= 2;
        }
        if(cnt1==cnt2) break;
        
        answer++;
    }
        
        return answer;
    }
    
}
```  
   
## 👊🏻 전략  
  
while문만 알면 풀 수 있다..
  
끝-!
