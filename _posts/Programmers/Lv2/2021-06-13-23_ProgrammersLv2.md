---
title: "[프로그래머스] [LV2] 2개 이하로 다른 비트"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-13 14:45:20
---

# 📚 2개 이하로 다른 비트
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/77885>  
  
>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob23/23-1.png)
![이미지](/assets/images/Programmers/Lv2/prob23/23-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public String solution(String s) {
        String answer = "";
        
        ArrayList<Integer> list = new ArrayList<Integer>();
        
        String[] str = s.split(" ");
        
        for(String st : str){
            if(st.charAt(0) =='-')
             list.add(-1 * Integer.parseInt(st.substring(1,st.length())));   
            else list.add(Integer.parseInt(st));
        }
        
        Collections.sort(list);
        
        answer = Integer.toString(list.get(0)) + " " + Integer.toString(list.get(list.size()-1));
        
        return answer;
    }
}
```  
   
## 👊🏻 전략  
  
1. split
2. - 부호 처리
3. 정렬 후 맨 앞/뒤 출력
  
끝-!
