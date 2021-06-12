---
title: "[프로그래머스] [LV2] 이진 변환 반복하기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-12 20:15:20
---

# 📚 이진 변환 반복하기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/70129>  
  
>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob19/19-1.png)
![이미지](/assets/images/Programmers/Lv2/prob19/19-2.png)
![이미지](/assets/images/Programmers/Lv2/prob19/19-3.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int[] solution(String s) {
        int[] answer = new int[2];
        int len = 0;
        ArrayList<Integer> list = new ArrayList<Integer>();
        
        for(int i = 0; i < s.length(); i++)
            list.add(s.charAt(i) - '0');
        
        while(true){
            answer[0]++;
            while(true){ // 1. 0을 모두 제거
                if(list.indexOf(0) == -1)break;
                else {
                    list.remove(list.indexOf(0));
                    answer[1]++;
                }
            }
            if(list.size() == 1) break;
            else{ // 2. 길이를 이진법으로 표현하는 함수
                len = list.size();
                list.clear();
                while(len!=0){
                    list.add(0, len%2);
                    len /= 2;
                }                
            }
        }
         
        return answer;
    }
}
```  
   
## 👊🏻 전략  
  
역할을 2가지로 나눠서 풀자  
1. 0을 모두 제거해주는 역할
2. 길이를 이진법으로 표현해주는 역할
  
끝-!
