---
title: "[프로그래머스] [LV2] 순위 검색"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - permutation
  - binarysearch
last_modified_at: 2021-08-01 23:30:20
---

# 📚 순위 검색
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/72412>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob56/56-1.png)
![이미지](/assets/images/Programmers/Lv2/prob56/56-2.png)
![이미지](/assets/images/Programmers/Lv2/prob56/56-3.png)
![이미지](/assets/images/Programmers/Lv2/prob56/56-4.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    
    static HashMap<String, List<Integer>> infomap;
    
    public int[] solution(String[] info, String[] query) {
        int[] answer = new int[query.length];
        
        infomap = new HashMap<>();
        
        for(int i = 0; i < query.length; i++){
            query[i] = query[i].replaceAll("and", "");
            query[i] = query[i].replaceAll(" ", "");
        }
       
        for(int i = 0; i < info.length; i++){
            String[] temp = info[i].split(" ");
            Permutation(0, temp, "");
        }
        
        for (Map.Entry<String, List<Integer>> entry : infomap.entrySet()) {
            Collections.sort(entry.getValue());
        }
        
        for(int i = 0; i < query.length; i++) {
            String[] temp = query[i].split("[0-9]"); // key
            String[] temp2 = query[i].split("[^0-9]"); // value
            
            int score = Integer.valueOf(temp2[temp2.length-1]);
            
            if(infomap.containsKey(temp[0])==false) answer[i] = 0;
            else{
                List<Integer> list = infomap.get(temp[0]);
                
                int start = 0, end = list.size()-1;
                
                while(start <= end){
                    int mid = (start + end) / 2;
                    if(list.get(mid) < score){
                        start = mid + 1;
                    }else{
                        end = mid - 1;
                    }
                }
                answer[i] = list.size() - start;
            }
        }
        return answer;
    }
   
    static void Permutation(int idx, String[] tmp, String result){
        
        if(idx >= 4){
            if(!infomap.containsKey(result)){
                List<Integer> list = new ArrayList<>();
                list.add(Integer.valueOf(tmp[4]));
                infomap.put(result,list);
            }
            else
                infomap.get(result).add(Integer.valueOf(tmp[4]));
            return;
        }
                
        Permutation(idx + 1, tmp, result + tmp[idx]);
        Permutation(idx + 1, tmp, result + "-");
    }
}
``` 
  
## 👊🏻 내 전략 
  
1. 순열을 사용해서 발생할 수 있는 케이스를 생성한다. 
2. map을 사용하여 점수(value)와 점수가 아닌 것(key)을 구분하여 저장한다.
3. 꺼내서 밸류보다 큰 값을 binarySearch로 찾는다.
  
끝-!
