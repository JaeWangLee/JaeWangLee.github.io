---
title: "[프로그래머스] [LV1] 키패드 누르기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-16 23:47:20
---

# 📚 키패드 누르기
  
링크📎 : <hhttps://programmers.co.kr/learn/courses/30/lessons/67256>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/23-1.png)
![이미지](/assets/images/Programmers/Lv1/23-2.png)
![이미지](/assets/images/Programmers/Lv1/23-3.png)
![이미지](/assets/images/Programmers/Lv1/23-4.png)

  
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public String solution(int[] numbers, String hand) {
        String answer = "";
        int [] left_Pos = {3, 0};
        int [] right_Pos = {3, 2};
        
        for(int i = 0; i < numbers.length; i++){
            answer += calcHand(numbers[i], hand, left_Pos, right_Pos);
        }
        
        return answer;
    }
    
    String calcHand(int n, String hand, int[] L_P, int[] R_P){
        int [] temp = {0,0};
        String temp_s ="";
        
        switch(n){
            case 1 :
                L_P[0] = 0;
                L_P[1] = 0;
                temp_s = "L";
                break;
            case 2 :
                temp[0] = 0;
                temp[1] = 1;
                break;
            case 3 :
                R_P[0] = 0;
                R_P[1] = 2;
                temp_s = "R";
                break;
            case 4 :
                L_P[0]  = 1;
                L_P[1]  = 0;
                temp_s = "L";
                break;
            case 5 :
                temp[0] = 1;
                temp[1] = 1;
                break;
            case 6 :
                R_P[0] = 1;
                R_P[1] = 2;
                temp_s = "R";
                break;
            case 7 :
                L_P[0]  = 2;
                L_P[1]  = 0;
                temp_s = "L";
                break;
            case 8 :
                temp[0] = 2;
                temp[1] = 1;
                break;
            case 9 :
                R_P[0] = 2;
                R_P[1] = 2;
                temp_s = "R";
                break;
            case 0 :
                temp[0] = 3;
                temp[1] = 1;
                break;
        }
        
        if(temp_s == ""){
            if(Math.abs(temp[0]-R_P[0]) + Math.abs(temp[1]-R_P[1])== Math.abs(temp[0]-L_P[0]) + Math.abs(temp[1]-L_P[1])){
                if(hand.equals("left")) {
                    temp_s = "L";
                    L_P[0] = temp[0];
                    L_P[1] = temp[1];
                }   
                else{
                    temp_s = "R";
                    R_P[0] = temp[0];
                    R_P[1] = temp[1];
                }
            }
            else if(Math.abs(temp[0]-R_P[0]) + Math.abs(temp[1]-R_P[1]) < Math.abs(temp[0]-L_P[0]) + Math.abs(temp[1]-L_P[1])){
                temp_s = "R";
                R_P[0] = temp[0];
                R_P[1] = temp[1];
            }
            else{
                temp_s = "L";
                L_P[0] = temp[0];
                L_P[1] = temp[1];
            }
        }
        return temp_s;
    }
}
```
어쩌다 보니 하드코딩으로 풀었다.  
더 깔끔하게 풀 수 있을 거 같은데, 
밤이 늦었으니 나중에 정리하는 걸로..  
  
끝-!
