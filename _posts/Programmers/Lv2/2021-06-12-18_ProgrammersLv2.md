---
title: "[프로그래머스] [LV2] 삼각 달팽이"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-12 20:00:20
---

# 📚 삼각 달팽이
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/68645>  
  
>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob18/18-1.png)
![이미지](/assets/images/Programmers/Lv2/prob18/18-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int[] solution(int n) {
        int[] answer = {};
        int xpos = 0;
        int ypos = 0;
        int num = 1;
        int mode = 1; 
        int[][] arr = new int[n][n];
        ArrayList<Integer> list = new ArrayList<Integer>();
        
        for(int[] i : arr)
            Arrays.fill(i, 0);
        
        //아래, 오, 위왼,아래,오, 위왼,   
        for(int i = n; i > 0; i--){
            for(int j = 0; j < i; j++){
                if(mode == 1){
                    arr[ypos][xpos] = num++;
                    if(j == i-1) {mode = 2; xpos++;}
                    else ypos++;
                }
                else if(mode == 2){
                    arr[ypos][xpos] = num++;
                    if(j == i-1) {mode = 3; xpos--; ypos--;}
                    else xpos++;
                }
                else if(mode == 3){
                    arr[ypos][xpos] = num++;
                    if(j == i-1) {mode = 1; ypos++;}
                    else {ypos--;xpos--;}
                }
            }
        }
        
        for(int[] i : arr)
            for(int j = 0; j < i.length; j++){
                if(i[j] == 0) break;
                list.add(i[j]);
            }
        
        answer = new int[list.size()];
        
        for(int i = 0; i < list.size(); i++)
            answer[i] = list.get(i);
        
        return answer;
    }
}
```  
   
## 👊🏻 전략  
  
1. n, n-1, n-2 ... 이런식으로 아래 / 오른쪽 / 왼쪽위 방향 바꿔 가면서 기입된다.
   - 숫자 카운트
   - 방향 전환(모드로 구분)
  
끝-!
