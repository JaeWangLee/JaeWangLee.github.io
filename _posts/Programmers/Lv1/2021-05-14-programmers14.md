---
title: "[프로그래머스] [LV1] 로또의 최고 순위와 최저 순위"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-14 21:11:20
---

# 📚 모의고사
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/77484>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/14-1.png)
![이미지](/assets/images/Programmers/Lv1/14-2.png)
![이미지](/assets/images/Programmers/Lv1/14-3.png)
![이미지](/assets/images/Programmers/Lv1/14-4.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int[] solution(int[] lottos, int[] win_nums) {
        
        int[] count = {0,0};
        int[] answer = {0,0};
        
        for(int i = 0; i < lottos.length; i++){
            for(int j = 0; j < win_nums.length; j++){
                if(lottos[i] == 0) {
                    count[0]++;
                    break;
                }
                else if(lottos[i] == win_nums[j])
                {
                    count[0]++;
                    count[1]++;
                }
            }
        }
        
        for(int i = 0; i < 2; i++){
            if(count[i] == 6) answer[i] = 1;
            else if(count[i] == 5) answer[i] = 2;
            else if(count[i] == 4) answer[i] = 3;
            else if(count[i] == 3) answer[i] = 4;
            else if(count[i] == 2) answer[i] = 5;
            else answer[i] = 6;
        }
        
        
        return answer;
    }
}
```
문제가 길어서 당황했는데.. 실상은 1도 어렵지 않은.. 문제  
다양한 노가다 풀이법이 존재하는 듯 하다.😅  
  
끝-!