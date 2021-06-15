---
title: "[프로그래머스] [LV2] 구명보트"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-15 22:08:20
---

# 📚 구명보트
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42885>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob28/28-1.png)
![이미지](/assets/images/Programmers/Lv2/prob28/28-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(int[] people, int limit) {
        int answer = 0;
        int sum = 0;
        
        ArrayList<Integer> list = new ArrayList<Integer>();
        
        Arrays.sort(people);

        for(int i : people)
            list.add(i);
        
        while(list.size()!=0){
            if(list.get(list.size()-1)*2<=limit) break;
            if(list.size() == 1||list.get(0) + list.get(list.size()-1) > limit){
                list.remove(list.size()-1);
            }
            else{
                list.remove(0);
                list.remove(list.size()-1);
            }
            answer++;
        }
        
        answer += list.size()%2==0 ? list.size()/2 : list.size()/2+1;
        
        return answer;
    }
}
```
  
## 👊🏻 내 전략
  
1. 최대 두명 탈 수 있음을 기억하자 ..
   - 이거 제대로 안봤다가 첨부터 다시 풀었다.😭
2. 인자로 주어지는 people의 크기가 크다.
   - sort 작업을 최소화 하자. 시간 초과 난다.
3. 현재 기준 가장 큰 몸무게의 2배가 limit 안에 들어오면 Break
   - 남은 case를 일일히 진행할 필요 없어진다.
   - 남은 크기의 절반 혹은 절반 + 1 만큼의 case를 더해주면 된다.
   
끝-!
