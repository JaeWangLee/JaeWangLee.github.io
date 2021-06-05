---
title: "[프로그래머스] [LV2] 문자열 압축"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-05 23:46:20
---

# 📚 문자열 압축
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/60057>  

>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob8/8-1.png)
![이미지](/assets/images/Programmers/Lv2/prob8/8-2.png)
![이미지](/assets/images/Programmers/Lv2/prob8/8-3.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(String s) {
        int answer = 0;
        List<String> list = new ArrayList(); 
        
        for(int i = 1; i <= s.length()/2; i++){
            String temp = "";
            int j = 0;
            int count = 1;
            list.add("");
            
            while(true)
            {
                if(i*(j+1) > s.length()){
                    if(count == 1)
                        list.set(list.size()-1, list.get(list.size()-1) + temp);
                    else
                        list.set(list.size()-1, list.get(list.size()-1) + Integer.toString(count) + temp);
                    
                    list.set(list.size()-1, list.get(list.size()-1) + s.substring(i*j,s.length()));
                    break;
                }
                
                if(temp.equals(s.substring(i*j,i*(j+1))))
                    count++;
                else{
                    if(count == 1)
                        list.set(list.size()-1, list.get(list.size()-1) + temp);
                    else
                        list.set(list.size()-1, list.get(list.size()-1) + Integer.toString(count) + temp);
                    temp = s.substring(i*j,i*(j+1));
                    count = 1;
                }
                j++;
            }          
        }
        
        int size = s.length();
        
        for(String st : list){
            if(st.length() < size) size = st.length();
        }
            
        answer = size;
        
        return answer;
    }
}
```  
  
끝-!
