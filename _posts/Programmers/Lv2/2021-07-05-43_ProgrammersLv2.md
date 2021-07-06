---
title: "[프로그래머스] [LV2] [1차] 프렌즈4블록"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-07-05 16:30:20
---

# 📚 [1차] 프렌즈4블록
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/17679>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob43/43-1.png)
![이미지](/assets/images/Programmers/Lv2/prob43/43-2.png)
![이미지](/assets/images/Programmers/Lv2/prob43/43-3.png)
![이미지](/assets/images/Programmers/Lv2/prob43/43-4.png)

## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(int m, int n, String[] board) {
        int answer = 0;
        
        Character[][] cboard = new Character[m][n];
        boolean[][] chk = new boolean[m][n];
        int temp = 0;
        
        for(boolean a[] : chk)
            Arrays.fill(a,false);
        
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++)
                cboard[i][j] = board[i].charAt(j);
        
        while(true){
           
            for(int i = 0; i < m - 1; i++){
                for(int j = 0; j < n - 1; j++){

                    if(cboard[i][j] == '0') continue;

                    if(!cboard[i][j].equals(cboard[i][j+1])) continue;
                    if(!cboard[i][j].equals(cboard[i+1][j])) continue;
                    if(!cboard[i][j].equals(cboard[i+1][j+1])) continue;

                    chk[i][j] = true;
                    chk[i+1][j] = true;
                    chk[i][j+1] = true;
                    chk[i+1][j+1] = true;
                }
            }

            for(int i = 0; i < m; i++){
                for(int j = 0; j < n; j++){
                    if(chk[i][j]) {
                        chk[i][j] = false;
                        cboard[i][j] = '0';
                        temp++;
                    }
                }
            }

            if(temp == 0) break;
            else {
                answer += temp;
                temp = 0;
            }
            
            for(int i = m - 1; i > 0; i--){
                for(int j = 0; j < n; j++){
                    while(cboard[i][j] == '0'){
                        int cnt = 0;
                        for(int k = i; k > 0; k--){
                            if(cboard[k-1][j] != '0') cnt++;
                            cboard[k][j] = cboard[k-1][j];
                        }
                        cboard[0][j] = '0';
                        if(cnt == 0) break;
                    }
                }
            }
            
        }
        
        return answer;
    }
}
``` 
  
## 👊🏻 내 전략 
  
나는 아직 알고리즘이나 자료구조를 배우지 못해서 그런지.. 이런 구현 문제가 더 재미있는 것 같다  
하나씩 구현해 나가면서 확인하고 고치면서 풀어나가는.. 근데 레벨이 오를 수록  
이런 문제가 점점 없어지는 것 같아 아쉽다😭  
  
1. string[]를 2차원의 character로 바꿔준다.
2. 지워줄 애들을 체크할 boolean 2차원 배열도 선언해준다.
3. 체크한 거 지워주고, 빈 공백없이 내려서 채워주면 끝!
  
끝-!
