---
title: "[프로그래머스] [LV1] 행렬의 덧셈"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-15 20:00:20
---

# 📚 모의고사
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12950?language=java>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/16-1.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public int[][] solution(int[][] arr1, int[][] arr2) {
        int[][] answer = new int[arr1.length][arr1[0].length];
        
        for(int i = 0; i < arr1.length; i++)
            for (int j = 0; j < arr1[0].length; j++)
               answer[i][j] = arr1[i][j] + arr2[i][j];             
                
        return answer;
    }
}
```
문제는 간단하지만, 배열에 대한 중요한 개념을 담고 있는 듯 하다.  
배열의 크기에 대해 알아보자. 

## 배열의 크기

일반적으로 배열(arr[x])의 크기는 `arr.length`로 구할 수 있다.  
> cf. 문자열(String)의 크기는 `str.length()`로 구한다.
  
**2차원 배열의 경우 어떨가?**  
`arr[3][4]`가 있을때  
1. 행의 크기를 구하려면?
   - `arr.length` 를 사용한다.  
   - 3이 출력됨
2. 열의 크기를 구하려면?
   - `arr[0].length`를 사용한다.
   - 4가 출력됨
  
끝-!