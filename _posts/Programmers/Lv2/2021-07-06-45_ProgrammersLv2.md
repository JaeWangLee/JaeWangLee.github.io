---
title: "[프로그래머스] [LV2] 점프와 순간 이동"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-07-06 21:30:20
---

# 📚 점프와 순간 이동
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12980>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob45/45-1.png)
![이미지](/assets/images/Programmers/Lv2/prob45/45-2.png)

## 📝 내 풀이  
  
```java  
import java.util.*;

public class Solution {
    public int solution(int n) {
        int ans = 0;
        
        while(n > 0){
            ans += n%2;
            n/=2;
        }
        
        return ans;
    }
}
``` 
  
## 👊🏻 내 전략 
  
1. 텔레포트를 많이 써야한다.  
2. 점프는? 텔레포트를 못할때만  
3. 텔레포트를 못쓸때는? 2로 나눠떨어지지 않을때  
4. n을 계속 2로 나눠주되, 나머지가 1인 경우 1씩 더 해준다.  
  
끝-!
