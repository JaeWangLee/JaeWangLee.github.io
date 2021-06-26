---
title: "[프로그래머스] [LV2] 방문 길이"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - map
last_modified_at: 2021-06-22 22:01:20
---

# 📚 방문 길이
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/49994>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob35/35-1.png)
![이미지](/assets/images/Programmers/Lv2/prob35/35-2.png)
![이미지](/assets/images/Programmers/Lv2/prob35/35-3.png)
![이미지](/assets/images/Programmers/Lv2/prob35/35-4.png)
![이미지](/assets/images/Programmers/Lv2/prob35/35-5.png)

## 📝 내 풀이  
    
```java  
import java.util.*;

class Solution {
    public int solution(String dirs) {
        int answer = 0;
        int xpos = 0;
        int ypos = 0;
        String d1_str = "";
        String d2_str = "";
        
        HashMap<String, Integer> map = new HashMap<String, Integer>();
        
        for(int i = 0; i < dirs.length(); i++){
            d1_str += Integer.toString(xpos);
            d1_str += Integer.toString(ypos);
            
            if(dirs.charAt(i) == 'U'){
                ypos++;
                if(ypos > 5) {
                    ypos = 5;
                    d1_str = "";
                    continue;
                }
            }
            else if(dirs.charAt(i) == 'D'){     
                ypos--;
                if(ypos < -5) {
                    ypos = -5;
                    d1_str = "";
                    continue;
                }
            }
            else if(dirs.charAt(i) == 'R'){
                xpos++;
                if(xpos > 5){
                    xpos = 5;
                    d1_str = "";
                    continue;
                }
                
            }
            else if(dirs.charAt(i) == 'L'){
                xpos--;
                if(xpos < -5){
                    xpos = -5;
                    d1_str = "";
                    continue;
                }
            }
            d2_str += Integer.toString(xpos);
            d2_str += Integer.toString(ypos);
            map.put(d1_str + d2_str, 1);
            map.put(d2_str + d1_str, 1);
            d1_str = "";
            d2_str = "";
        } 
        
        answer = map.size()/2;
        
        return answer; 
    }
}
``` 
   
## 👊🏻 내 전략
  
1. Map의 Key속성을 이용하자
2. x1y1x2y2 이런식으로 key를 정리하되 반대 방향도 고려하자
   x2y2x1y1 이런식으로!
3. 답은 key의 길이의 반을 출력한다~
   
  
끝-!
