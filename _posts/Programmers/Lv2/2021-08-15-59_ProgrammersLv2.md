---
title: "[프로그래머스] [LV2] 메뉴 리뉴얼"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - java
  - 자바
  - map
last_modified_at: 2021-08-15 18:00:20
---

# 📚 메뉴 리뉴얼
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/72411>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob59/59-1.png)
![이미지](/assets/images/Programmers/Lv2/prob59/59-2.png)
![이미지](/assets/images/Programmers/Lv2/prob59/59-3.png)
![이미지](/assets/images/Programmers/Lv2/prob59/59-4.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    
    static int m;
    static HashMap<String, Integer> map; 
        
    public String[] solution(String[] orders, int[] course) {
        PriorityQueue<String> pq = new PriorityQueue<>();

        for(int i = 0; i < course.length; i++){
            m = 0;
            map = new HashMap<>();

            for(int j = 0; j < orders.length; j++){
                find(0, "", course[i], 0, orders[j]);
            }
            
            for (String s : map.keySet()){
                if (map.get(s)==m&&m>1){
                    pq.offer(s);
                }
            }
        }
        
        String  answer[] = new String[pq.size()];
        int k=0;
        
        while (!pq.isEmpty()){
            answer[k++] = pq.poll();
        }
        
        return answer;
    }
    
    static void find(int cnt, String str, int targetNum, int idx, String word){
        if(cnt == targetNum){
            char[] c = str.toCharArray();
            Arrays.sort(c);
            String temps = "";
            
            for(int i = 0; i<c.length; i++) temps += c[i];
            
            map.put(temps,map.getOrDefault(temps,0)+1);
            m = Math.max(m,map.get(temps));
            
            return;
        }
        for (int i=idx;i<word.length();i++){
            char now =word.charAt(i);
            find(cnt+1,str+now,targetNum,i+1,word);
        }
    }
}
``` 
  
## 👊🏻 내 전략 
  
이 문제는 조건이 명확하게 명시되있지 않아 어려웠었다.  
아래의 조건을 추가하고 풀이를 하겠다.  
- 같은 길이의 course에서는 가장 많이 주문된 메뉴 구성만 추가한다. 
  
1. 중복제거를 위해 Hash를 써야하고,
2. Map을 사용해서 메뉴구성과 몇번 주문되었는지 체크한다.
3. map.getOrDefault()와 Priorityqueue를 사용해서 효율성을 높인다. 
  
끝-!
