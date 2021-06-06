---
title: "[프로그래머스] [LV2] 조이스틱"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-05 19:35:20
---

# 📚 조이스틱
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42860>  

>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob6/6-1.png)
![이미지](/assets/images/Programmers/Lv2/prob6/6-2.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public int solution(String name) {
        int answer = 0;
        int temp = 0;
        int buf = 0;
        
        for(int i = 0; i < name.length(); i++){
          answer += name.charAt(i) - 'A' <= 'Z' + 1 - name.charAt(i) ? name.charAt(i) - 'A':'Z' + 1 - name.charAt(i);  
        }
        
        int minMove = name.length() - 1; 
        for(int i = 0 ; i < name.length() ; i++) {
            if(name.charAt(i) != 'A') {
                int next = i+1;

                while(next < name.length() && name.charAt(next) == 'A') 
                    next++;
                
                int move = 2 * i + name.length() - next;

                minMove = Math.min(move, minMove);
            }
        }

        return answer + minMove;
    }
}
```  
  
## 👊🏻 전략  
  
0. 두가지로 나눠서 풀자
1. 원하는 알파벳으로 바꾸는 방법
   - 위로 가는 방향
   - 아래로 가는 방향
2. 원하는 위치로 옮기는 방법
   - 왼쪽으로 가는 방향
   - 오른쪽으로 가는 방향
  
1은 쉽게 구현 가능하고,  
2도 단반향으로 가는게 제일 짧은 거리라 생각을 했지만,  
그렇게 하면 테케 마지막에서 걸리게 된다.  
  
오른쪽으로가다가 왼쪽으로 가는 경우가 있기 때문..  
총 3가지로 나눠서 생각해서 풀면 되겠다.  
  
끝-!
