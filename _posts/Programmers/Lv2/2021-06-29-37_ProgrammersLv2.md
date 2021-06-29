---
title: "[프로그래머스] [LV2] 행렬 테두리 회전하기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-29 20:05:20
---

# 📚 행렬 테두리 회전하기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12949>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob37/37-1.png)

## 📝 내 풀이  
    
```java  
class Solution {
      public int[][] solution(int[][] arr1, int[][] arr2) {
          int[][] answer = new int[arr1.length][arr2[0].length];

          for(int i = 0 ; i < arr1.length ; ++i){
              for(int j = 0 ; j < arr2[0].length ; ++j){
                  for(int k = 0 ; k < arr1[0].length ; ++k) {
                      answer[i][j] += arr1[i][k] * arr2[k][j];
                  }
              }
          }

          return answer;
      }
  }
``` 
   
## 👊🏻 내 전략
  
머리로만 생각해서 코딩을 할려하니 잘 정리가 되지 않았다.  
행렬 연산에 대해서 식을 써보고 그에 맞게 끔 포멧 정리를 해보았다.  
2x2행렬이 있다고 한다면  
answer[1][1] = answer[0][0] x answer[0][0] + answer[0][1] x answer[1][0]  
이런식으로 된다. 정리를 해보면,  
answer[i][j] += arr1[i][k] * arr2[k][j]; 이 도출되고 그에 맞게 끔 for문을 작성하였다.  
  
끝-!
