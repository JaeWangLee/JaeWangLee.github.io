---
title: "[프로그래머스] [LV2] 오픈채팅방"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-24 22:30:20
---

# 📚 오픈채팅방
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42888>  

>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob2/2-1.png)
![이미지](/assets/images/Programmers/Lv2/prob2/2-2.png)
![이미지](/assets/images/Programmers/Lv2/prob2/2-3.png)
![이미지](/assets/images/Programmers/Lv2/prob2/2-4.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public String[] solution(String[] record) {
        String[] answer = {};
        String[] act = new String[record.length];
        String[] temp = new String[3];
        
        ArrayList<String> list = new ArrayList<String>();
        HashMap<String,String> map = new HashMap<String,String>();
        
        for(String s : record){
            temp = s.split(" ");
            if(temp[0].equals("Enter")){
                map.put(temp[1], temp[2]);
            }
            else if(temp[0].equals("Change"))
                map.put(temp[1], temp[2]);
        }
        
        
        
        for(int i = 0; i < record.length; i++){
            temp = record[i].split(" ");
            
            if(temp[0].equals("Enter"))
                list.add(map.get(temp[1]) + "님이 들어왔습니다.");
            else if(temp[0].equals("Leave"))
                list.add(map.get(temp[1]) + "님이 나갔습니다.");
        }
        
        answer = new String[list.size()];
        
        for(int i = 0; i < list.size(); i++)
            answer[i] = list.get(i);
        
            
        return answer;
    }
}
```  
  
끝-!
