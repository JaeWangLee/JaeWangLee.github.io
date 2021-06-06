---
title: "[프로그래머스] 프린터"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-06 14:06:20
---

# 📚 프린터
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42587?language=java>  

>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob10/10-1.png)
![이미지](/assets/images/Programmers/Lv2/prob10/10-2.png)
  
## 📝 내 풀이  
  
```java  import java.util.*;

class Solution {
    public int solution(int[] priorities, int location) {
        int answer = 0;
        int maxNum = 1;
        int temp = 0;
        
        List<Integer> val_list = new ArrayList<Integer>();
        List<Integer> loc_list = new ArrayList<Integer>();

        for(int i = 0; i < priorities.length; i++){
            if(maxNum < priorities[i]) maxNum = priorities[i];
            val_list.add(priorities[i]);
            if(i == location)
                loc_list.add(1);
            else
                loc_list.add(0);
        }            

        while(!val_list.isEmpty()){
            
            if(!val_list.contains(maxNum)) {
                maxNum--;
                continue;
            }
                
            if(val_list.get(0)==maxNum){
                answer++;
                val_list.remove(0);
                if(loc_list.get(0) == 1) break;
                else
                    loc_list.remove(0);                
            }
            else{
                temp = val_list.get(0);
                val_list.remove(0);
                val_list.add(temp);
                
                temp = loc_list.get(0);
                loc_list.remove(0);
                loc_list.add(temp);
            }
        }
        
        return answer;
    }
}
```  
  
## 👊🏻 전략  
  
1. 두개의 List를 구현한다.
   - 하나는 벨류용
   - 하나는 제거하고자 하는 언소 위치 파악용
2. maxNum을 두고 존재 여부를 파악해서 Update 하자.
  
끝-!
