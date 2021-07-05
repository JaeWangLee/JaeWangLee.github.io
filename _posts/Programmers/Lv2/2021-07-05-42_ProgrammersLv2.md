---
title: "[프로그래머스] [LV2] [3차] 방금그곡"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - linkedhashmap
  - map
last_modified_at: 2021-07-05 13:50:20
---

# 📚 [3차] 방금그곡
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/17683>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob42/42-1.png)
![이미지](/assets/images/Programmers/Lv2/prob42/42-2.png)

## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public String solution(String m, String[] musicinfos) {
        String answer = "";
        
        LinkedHashMap<String, String> map = new LinkedHashMap<String, String>();
        Queue<String> q = new LinkedList<String>();
        
        m = m.replace("C#", "H").replace("D#", "I").replace("F#", "J").replace("G#", "K").replace("A#", "L");
        
        for(int i = 0; i < musicinfos.length; i++){
            
            musicinfos[i] = musicinfos[i].replace("C#", "H").replace("D#", "I").replace("F#", "J").replace("G#", "K").replace("A#", "L");;
            
            String[] temp = musicinfos[i].split(",");
            String[] len_stemp = (temp[0] + ":" + temp[1]).split(":");            
            
            int len = (Integer.parseInt(len_stemp[2]) - Integer.parseInt(len_stemp[0])) * 60 + (Integer.parseInt(len_stemp[3])- Integer.parseInt(len_stemp[1]));    
           
            
            
            if(temp[3].length() > len){
                map.put(temp[2],temp[3].substring(0, len));
            }
            else{
                int portion = len / temp[3].length();
                int rest = len % temp[3].length();
                map.put(temp[2],temp[3].repeat(portion) + temp[3].substring(0,rest));
            }
        }
        
        for( String key : map.keySet() ){
            if(map.get(key).contains(m)) q.offer(key);
        }
        
        if(q.isEmpty()) answer = "(None)";
        else if(q.size() == 1) answer = q.poll();
        else{
            answer = q.poll();
            while(!q.isEmpty()){
                if(map.get(answer).length() < map.get(q.peek()).length()) answer = q.poll();
                else q.poll();
            }
        }
        
        return answer;
    }
}
``` 
  
## 👊🏻 내 전략 
  
1. C# 같이 두 글자수를 먹는 경우는 처리하기 까다롭다.
   - 미리 다른 문자로 치환하자
2. Map의 Key-Value를 활용해서 문제를 풀어가자
   - 단, Map은 순서가 보장안되므로, LinkedHashMap을 사용하
3. 여러 후보가 있을 수 있으니 queue를 이용해서 넣어두고 나중에 비교하자.
  
끝-!
