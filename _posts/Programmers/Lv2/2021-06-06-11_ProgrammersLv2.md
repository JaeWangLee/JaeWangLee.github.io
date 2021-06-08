---
title: "[프로그래머스] [LV2] 튜플"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - hashmap
  - hashmap정렬
last_modified_at: 2021-06-06 15:50:20
---

# 📚 프린터
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/64065>  
  
>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob11/11-1.png)
![이미지](/assets/images/Programmers/Lv2/prob11/11-2.png)
![이미지](/assets/images/Programmers/Lv2/prob11/11-3.png)
![이미지](/assets/images/Programmers/Lv2/prob11/11-4.png)
  
## 📝 내 풀이  
  
```java  
  
import java.util.*;

class Solution {
    public int[] solution(String s) {
        int[] answer = {};
        HashMap<Integer, Integer> hm = new HashMap<Integer, Integer>();
        
        s = s.substring(1, s.length()-1);
        
        while(s.length()!=1){
            String[] temp = s.substring(1,s.indexOf("}")).split(",");
            for(String s1 : temp){
                if(!hm.containsKey(Integer.parseInt(s1)))
                    hm.put(Integer.parseInt(s1),1);
                else
                    hm.put(Integer.parseInt(s1), hm.get(Integer.parseInt(s1)) + 1);
            }
            s = s.substring(s.indexOf("}"), s.length());
            if(s.length()==1) break;
            else s = s.substring(s.indexOf("{"), s.length());
        }

        List<Integer> keySetList = new ArrayList<>(hm.keySet());
		
		// 내림차순
		Collections.sort(keySetList, (o1, o2) -> (hm.get(o2).compareTo(hm.get(o1))));
		
        answer = new int[keySetList.size()];
        int count = 0;
        
		for(Integer key : keySetList) {
			answer[count++] = key;
		}
        
        return answer;
    }
}
```  
  
풀이가 너무 더럽다..  

## 👊🏻 전략  
  
1. split으로 각 원소별로 끊는다.
2. 최빈 원소일 수록 앞으로 배치되면 된다.
3. hashmap의 키값으로 원소를 넣고 value로 빈도수 체크
4. 내림차순으로 정렬하여 반환한다. 
  
끝-!
