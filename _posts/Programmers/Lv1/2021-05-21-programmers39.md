---
title: "[프로그래머스] [LV1] 자릿수 더하기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 23:42:20
---

# 📚 자릿수 더하기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12931>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/39-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

public class Solution {
    public int solution(int n) {
        int answer = 0;
        
        while(n!=0){
            answer += n%10;
            n = n/10;
        }
        
        return answer;
    }
}
```  
  
끝-!
