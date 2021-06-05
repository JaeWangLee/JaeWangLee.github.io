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
        
        // 두가지로 나눠서 풀자
        // 1. 원하는 알파벳으로 바꾸는 방법
        // 2. 원하는 위치로 옮기는 방법
        // 1 은 위로 가는 방향, 아래로 가는 방향을 고려하고
        // 2 는 왼쪽으로 가는 방향, 오른쪽으로 가는 방향을 고려한다.
        // 왜냐면 왼쪽 갔다 오른쪽 가거나, 위로 갔다 아래로 가면 무조건 최솟값이 아니기 때문에
        
        // 1에 대해서 먼저 해보자
        for(int i = 0; i < name.length(); i++){
          answer += name.charAt(i) - 'A' <= 'Z' + 1 - name.charAt(i) ? name.charAt(i) - 'A':'Z' + 1 - name.charAt(i);         
        }
        
        // 2에 대해서 해보자
        int minMove = name.length() - 1; // 이거 이상 움직일 수 없음. Max 값
        for(int i = 0 ; i < name.length() ; i++) {
            if(name.charAt(i) != 'A') {
                int next = i+1;

                while(next < name.length() && name.charAt(next) == 'A') // 최대가 넘지 않는 선에서 오른쪽으로 쭈욱 감.
                    next++;
                
                int move = 2 * i + name.length() - next;

                minMove = Math.min(move, minMove);
            }
        }

        return answer + minMove;
    }
}
```  
  
끝-!
