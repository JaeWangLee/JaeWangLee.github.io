---
title: "[프로그래머스] [LV1] 크레인 인형뽑기 게임"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-16 15:33:20
---

# 📚 크레인 인형뽑기 게임
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/64061>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/21-1.png)
![이미지](/assets/images/Programmers/Lv1/21-2.png)
![이미지](/assets/images/Programmers/Lv1/21-3.png)
![이미지](/assets/images/Programmers/Lv1/21-4.png)
![이미지](/assets/images/Programmers/Lv1/21-5.png)
![이미지](/assets/images/Programmers/Lv1/21-6.png)
  
문제가 매우 길다..  
  
## 📝 내 풀이  
  
```java  
import java.util.*; 
 
class Solution {
    public int solution(int[][] board, int[] moves) {
        int answer = 0;
        int temp = 0;
        ArrayList<Integer> basket = new ArrayList<Integer>();
        
        basket.add(clawMachine(board, moves[0]));
        
        for(int i = 1; i < moves.length; i++){
            temp = clawMachine(board, moves[i]);
            if(temp != 0){
                if((!basket.isEmpty()) && (temp == basket.get(basket.size()-1))){
                    basket.remove(basket.size()-1);
                    answer += 2;
                    continue;
                }   
                basket.add(temp);
            }
        }
        
        return answer;
    }
    
    public int clawMachine(int[][] arr, int n){
        
        int dollNum = 0;
        
        for(int i = 0; i < arr[0].length; i++){
            if(arr[i][n-1]==0) continue;
            else{
                dollNum = arr[i][n-1];
                arr[i][n-1] = 0;
                break;
            }
        }
        
        return dollNum; 
    }
}
```
  
끝-!
