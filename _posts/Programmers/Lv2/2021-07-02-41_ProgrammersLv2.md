---
title: "[프로그래머스] [LV2] 가장 큰 정사각형 찾기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - dp
last_modified_at: 2021-07-02 17:50:20
---

# 📚 가장 큰 정사각형 찾기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12905#>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob41/41-1.png)
![이미지](/assets/images/Programmers/Lv2/prob41/41-2.png)

## 📝 내 풀이  
  
첫번째 풀이  
  
반복문을 활용하여 풀이하였다.  
정확도는 다 맞으나, 효율성에서 전부 시간 초과가 발생한다.  
  
```java  
class Solution
{
    public int solution(int [][]board)
    {
        int answer = 0;
        int len = 0;
        boolean chk = true;
        
        for(int i = 0; i < board.length;i++){
            for(int j = 0; j < board[0].length; j++){
                if(board[i][j] == 0) continue;
                else{
                    chk = true;
                    len = 0;
                    while(chk){
                        if(i+len > board.length -1 || j+len > board[0].length-1) break;
                        for(int k = 0; k <= len; k++){
                            if(board[i+k][j+len] == 0 || board[i+len][j+k]==0) {
                                chk = false;
                                break;
                            }
                        }
                        if(chk) len++;
                    }
                    if(answer < len) answer = len;
                }
            }    
        }
        answer = (int)Math.pow(answer,2);
        return answer;
    }
}
``` 
   
두번째 풀이  
  
dp를 이용한 풀이이다.  
현재 위치가 [i][j]라 할때 [i-1][j-1], [i-1][j], [i][j-1]위치의 값이 0이 아닌 값이라면,  
세개의 값 중 가장 작은 값에 +1을 한 값으로 업데이트 해준다.  
계속하다보면 가장 큰 값이 발생하게 된다.  
  
```java  
class Solution
{
    public int solution(int [][]board)
    {
        int answer = 0;
        int len = 0;
        boolean chk = true;
        
        for(int i = 0; i < board.length;i++){
            for(int j = 0; j < board[0].length; j++){
                if(board[i][j] == 0) continue;
                else if(i != 0 && j != 0)                 
                    board[i][j] = 1 + Math.min(board[i-1][j-1], Math.min(board[i][j-1],board[i-1][j]));
                
                if(answer < board[i][j]) answer = board[i][j];
            }    
        }
        return (int)Math.pow(answer,2);
    }
}
```
  
끝-!
