---
title: "[프로그래머스] [LV1] 체육복"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
- Programmers
tags:
- codingtest
- algorithm
last_modified_at: 2021-05-16 12:33:20
---

# 📚 체육복

링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42862?language=java#>  

>난이도 ⭐️

## 📖 문제  

![이미지](/assets/images/Programmers/Lv1/20-1.png)
![이미지](/assets/images/Programmers/Lv1/20-2.png)

## 📝 내 풀이  

```java  
import java.util.*;
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        int answer = 0;
        
        int [] student = new int[n];
        
        Arrays.fill(student, 1);
        
        for(int i = 0; i < lost.length; i++)
            student[lost[i] - 1]--;    
        
        for(int i = 0; i < reserve.length; i++)
            student[reserve[i] - 1]++;
        
        for(int i = 0; i < n; i++){
            if(student[i] >= 1) continue;
            else{
                if(i == 0){
                    if(student[1] >= 2){
                        student[0]++;
                        student[1]--;
                    }
                }
                else if(i == n-1){
                    if(student[n-2] >=2){
                        student[n-1]++;
                        student[n-2]--;
                    }
                }
                else{
                    if(student[i-1] >=2){
                        student[i]++;
                        student[i-1]--;
                    }
                    else if(student[i+1] >=2){
                        student[i]++;
                        student[i+1]--;
                    }
                }
            }
        }
        
        for(int i = 0; i < n; i++)
            if(student[i]>=1) answer++;
        
        return answer;
    }
}
```
## Greedy 알고리즘 (탐욕법)
설명하자.


끝-!