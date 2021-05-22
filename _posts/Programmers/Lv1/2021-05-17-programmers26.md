---
title: "[프로그래머스] [LV1] 실패율"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-17 21:30:20
---

# 📚 실패율
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42889>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/26-1.png)
![이미지](/assets/images/Programmers/Lv1/26-2.png)
![이미지](/assets/images/Programmers/Lv1/26-3.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(int n) {
        int answer = 0;        
        ArrayList<Integer> threeNum = new ArrayList<Integer>();
        
        while(n!=0){  
            threeNum.add(n%3);
            n /= 3;
        }
        
        for(int i = 0; i < threeNum.size(); i++){
             answer += threeNum.get(threeNum.size()-1-i)*Math.pow(3,i);
        }
        
        return answer;
    }
}

```  
  
끝-!
