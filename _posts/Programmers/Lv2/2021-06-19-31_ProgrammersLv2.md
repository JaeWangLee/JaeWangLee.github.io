---
title: "[프로그래머스] [LV2] 영어 끝말잇기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - hashmap
last_modified_at: 2021-06-19 15:48:20
---

# 📚 영어 끝말잇기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12981>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob31/31-1.png)
![이미지](/assets/images/Programmers/Lv2/prob31/31-2.png)
![이미지](/assets/images/Programmers/Lv2/prob31/31-3.png)
![이미지](/assets/images/Programmers/Lv2/prob31/31-4.png)

## 📝 내 풀이  
    
```java  
import java.util.*;

class Solution {
    public int[] solution(int n, String[] words) {
        int[] answer = {0,0};
        char comp = words[0].charAt(0);

        HashMap<String,Integer> map = new HashMap<String, Integer>();
        
        for(int i = 0; i < words.length; i++){
            
            if(map.containsKey(words[i]) || words[i].charAt(0) != comp){
                answer[0] = i%n + 1;
                answer[1] = i/n + 1;
                break;
            }
            else map.put(words[i],1);
            
            comp = words[i].charAt(words[i].length() - 1); 
        }

        return answer;
    }
}
```
  
## 👊🏻 내 전략
  
1. 중복 검색을 위해 map을 활용한다.
2. 끝말잇기가 되지 않는 경우를 체크하기 위해 앞뒤 문자 확인한다.
  
끝-!
