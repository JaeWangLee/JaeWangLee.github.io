---
title: "[프로그래머스] [LV2] [3차] 압축"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - queue
  - arraylist
last_modified_at: 2021-07-09 12:30:20
---

# 📚 [3차] 압축
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/17684>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob49/49-1.png)
![이미지](/assets/images/Programmers/Lv2/prob49/49-2.png)
![이미지](/assets/images/Programmers/Lv2/prob49/49-3.png)
![이미지](/assets/images/Programmers/Lv2/prob49/49-4.png)

## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int[] solution(String msg) {
        int[] answer = {};
        Queue<Integer> q = new LinkedList<Integer>();
        ArrayList<String> dictionary = new ArrayList<String>();        
                
        dictionary.add("0");
        
        for(char c = 'A'; c <= 'Z'; c++)
            dictionary.add(c+"");
        
        for(int i = 0; i < msg.length(); i++){
            for(int j = i+1; j <= msg.length(); j++){
                if(dictionary.indexOf(msg.substring(i,j)) == -1){
                    dictionary.add(msg.substring(i,j));
                    q.offer(dictionary.indexOf(msg.substring(i,j-1)));
                    i = j-2;
                    break;
                }
                else if(j == msg.length()){
                    q.offer(dictionary.indexOf(msg.substring(i,j)));
                    i = j-1;
                    break;
                }
            }
        }
        
        answer = new int[q.size()];
        int idx = 0;
        
        while(!q.isEmpty())
            answer[idx++] = q.poll();
        
        return answer;
    }
}
``` 
  
## 👊🏻 내 전략 
  
1. 단어를 관리할 dictionary는 ArrayList로 선언하였다.
2. Queue를 선언하여 인덱스를 관리하도록 하였다.
3. substring과 indexOf를 적절히 활용하여 단어를 도출하고, 검색하는데 사용하였다.
  
끝-!
