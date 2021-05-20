---
title: "[프로그래머스] [LV1] [1차] 다트 게임"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-20 23:45:20
---

# 📚 [1차] 다트 게임
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/17682?language=java>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/27-1.png)
![이미지](/assets/images/Programmers/Lv1/27-2.png)
![이미지](/assets/images/Programmers/Lv1/27-3.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(String dartResult) {
        int answer = 0;
        int count = -1;
        int[] score = new int[3];
        boolean isScore = true;
        
        for(int i = 0; i < dartResult.length(); i++){
            if(dartResult.charAt(i) >='0'&& dartResult.charAt(i) <= '9') isScore = true;
            
            if(isScore){
                count++;
                if(dartResult.charAt(i+1)=='0') {score[count] = 10; i++;}
                else score[count] = dartResult.charAt(i) - '0';
                isScore = false;
            }
            else{
                switch(dartResult.charAt(i)){
                    case 'S' :
                        score[count] = (int)Math.pow(score[count], 1);
                        break;
                    case 'D' :
                        score[count] = (int)Math.pow(score[count], 2);
                        break;
                    case 'T' :
                        score[count] = (int)Math.pow(score[count], 3);
                        break;
                    case '*' :
                        if(count != 0) {
                            score[count-1] = score[count-1]*2;
                            score[count] = score[count]*2;
                            }
                        else score[count] = score[count]*2;
                        break;
                    case '#' :
                        score[count] = -score[count];
                        break;
                }
            }
        }
        
        for(int i:score){
                answer += i;
            }
        
        
        
        
        return answer;
    }
}
```  
  
끝-!
