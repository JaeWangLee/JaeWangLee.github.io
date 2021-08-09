---
title: "[프로그래머스] [LV2] 쿼드압축 후 개수 세기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-08-09 17:00:20
---

# 📚 쿼드압축 후 개수 세기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/81302#>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob58/58-1.png)
![이미지](/assets/images/Programmers/Lv2/prob58/58-2.png)
![이미지](/assets/images/Programmers/Lv2/prob58/58-3.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    static int[] answer;
    public int[] solution(int[][] arr) {
        answer = new int[2];
        div(0, 0, arr.length, arr);
        return answer;
    }
    
    static void div(int x, int y, int len, int[][] arr){
        boolean isZero = true;
        boolean isOne = true;

        for(int i = x; i < x + len; i++){
            for(int j = y; j < y + len; j++){
                if(arr[i][j] == 1) isZero = false;
                if(arr[i][j] == 0) isOne = false;
                
                if(!isZero && !isOne) break;
            }
            
            if(!isZero && !isOne) break;
        }
        
        if(isZero){
            answer[0]++;
            return;
        }
        
        if(isOne){
            answer[1]++;
            return;
        }
        
        div(x,y,len/2,arr);
        div(x+len/2,y,len/2,arr);
        div(x,y+len/2,len/2,arr);
        div(x+len/2,y+len/2,len/2,arr);
        
    }
}
``` 
  
## 👊🏻 내 전략 
  
1. 하나라도 1이나 0이 있으면 1과 0으로 통합하지 못한다.
2. 통합하지 못한다면 그에 대해서 1/2의 크기로 분할한다.
3. 통합된다면 그대로 answer값을 조정해주고 return
4. 1과3를 반복한다(재귀)
  
끝-!
