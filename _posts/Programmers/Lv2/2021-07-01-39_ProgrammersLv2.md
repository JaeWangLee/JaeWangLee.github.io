---
title: "[프로그래머스] [LV2] 땅따먹기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - dp
last_modified_at: 2021-07-01 10:45:20
---

# 📚 땅따먹기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12913>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob39/39-1.png)

## 📝 내 풀이  
    
```java  
class Solution {
    int solution(int[][] land) {
        int answer = 0;

        for(int i = 1; i < land.length; i++){
            land[i][0] += Math.max(Math.max(land[i-1][1], land[i-1][2]), land[i-1][3]);
            land[i][1] += Math.max(Math.max(land[i-1][0], land[i-1][2]), land[i-1][3]);
            land[i][2] += Math.max(Math.max(land[i-1][1], land[i-1][0]), land[i-1][3]);
            land[i][3] += Math.max(Math.max(land[i-1][1], land[i-1][2]), land[i-1][0]);
        }
        
        for(int num : land[land.length-1])
            if(answer < num) answer = num;

        return answer;
    }
}
``` 
   
## 👊🏻 내 전략
  
1. 각 행에 대한 최댓값만 고려해야할 게 아니라, 이전 케이스에 대해서도 고려해야한다.  
2. 각 i행에 대해서 i-1행에서의 최댓값을 구해서 더해준다.(본인 열 제외) 
3. 마지막 행에서는 그동안에 더해진 최종 값이 나오고
4. 마지막 행에서 가장 큰 수를 도출해준다.
5. Dynamic Programming 기법이며, 나중에 다루도록 하겠다.
   
  
끝-!
