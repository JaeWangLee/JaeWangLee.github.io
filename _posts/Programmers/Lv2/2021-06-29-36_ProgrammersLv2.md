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
last_modified_at: 2021-06-29 20:01:20
---

# 📚 행렬 테두리 회전하기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/77485>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob36/36-1.png)
![이미지](/assets/images/Programmers/Lv2/prob36/36-2.png)
![이미지](/assets/images/Programmers/Lv2/prob36/36-3.png)
![이미지](/assets/images/Programmers/Lv2/prob36/36-4.png)
![이미지](/assets/images/Programmers/Lv2/prob36/36-5.png)
![이미지](/assets/images/Programmers/Lv2/prob36/36-6.png)

## 📝 내 풀이  
    
```java  
class Solution {
    public int[] solution(int rows, int columns, int[][] queries) {
        int[] answer = new int[queries.length];
        
        int[][] matrix = new int[rows][columns];
            
        for(int j = 0; j < matrix.length; j++){
            for(int k = 0; k < matrix[0].length; k++){
                matrix[j][k] = j*matrix[0].length + k + 1;
            }
        }
        
        for(int i = 0; i < queries.length; i++){
            
            int min = rows*columns;
            
            int x1 = queries[i][0] - 1;
            int y1 = queries[i][1] - 1;            
            int x2 = queries[i][2] - 1;
            int y2 = queries[i][3] - 1;
            
            int temp = matrix[x1][y2];
            
            for(int j = y2; j > y1; j--){
                matrix[x1][j] = matrix[x1][j-1];
                if(matrix[x1][j] < min) min = matrix[x1][j];
            }
                
            for(int j = x1; j < x2; j++){
                matrix[j][y1] = matrix[j+1][y1];
                if(matrix[j][y1] < min) min = matrix[j][y1];
            }
            
            for(int j = y1; j < y2; j++){
                matrix[x2][j] = matrix[x2][j+1];
                if(matrix[x2][j] < min) min = matrix[x2][j];
            }
            
            for(int j = x2; j > x1 ; j--){
                matrix[j][y2] = matrix[j-1][y2];
                if(matrix[j][y2] < min) min = matrix[j][y2];
            }
            
            matrix[x1+1][y2] = temp;
            if(matrix[x1+1][y2] < min) min = matrix[x1+1][y2];
                
            answer[i] = min;
        }
        
        
        
        return answer;
    }
}
``` 
   
## 👊🏻 내 전략
  
1. 순차적으로 값을 덮어쓰면서 행렬을 회전시킨다고 생각하자
2. 총 4방향으로 같은 방식으로 회전 시킨다.
3. 회전 시키면서 가장 작은 minimum 값을 찾는다.
   
  
끝-!
