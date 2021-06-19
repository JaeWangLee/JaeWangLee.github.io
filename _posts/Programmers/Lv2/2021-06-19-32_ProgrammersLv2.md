---
title: "[프로그래머스] [LV2] [3차] n진수 게임"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - stack
  - queue
last_modified_at: 2021-06-19 16:48:20
---

# 📚 [3차] n진수 게임
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/17687>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob32/32-1.png)
![이미지](/assets/images/Programmers/Lv2/prob32/32-2.png)

## 📝 내 풀이  
    
```java  
import java.util.*;

class Solution {
    public String solution(int n, int t, int m, int p) {
        String answer = "";
        int temp = 0;
        int currentNum = 0;
        int count = 0;
        int idx = 1;
        String[] table = {"0","1","2","3","4","5","6","7","8","9","A","B","C","D","E","F"};
        
        Stack<Integer> bufNum = new Stack<Integer>();
        Queue<String> qNum = new LinkedList<String>();
        
        p %= m;
        while(count < t){
            while(true){
                bufNum.push(temp % n);
                temp /= n;
                if(temp==0) break;
            }
            
            while(!bufNum.isEmpty() && count < t){
                if(idx % m == p){
                    qNum.offer(table[bufNum.pop()]);
                    count++;
                }
                else bufNum.pop();
                idx++;
            }
            temp = ++currentNum;
        }
        
        while(!qNum.isEmpty()){
            answer += qNum.poll();
        }
        
        return answer;
    }
}
```
  
## 👊🏻 내 전략
  
1. 진수 변환을 위한 bufNum, 정답 저장을 위핸 qNum을 선언한다.
2. 10이 넘는 경우 A, B,,,의 형태로 표현하는데 이것은 배열을 활용하였다. 
  
끝-!
