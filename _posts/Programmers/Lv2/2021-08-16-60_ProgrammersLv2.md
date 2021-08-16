---
title: "[프로그래머스] [LV2] 후보키"
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
last_modified_at: 2021-08-16 22:00:20
---

# 📚 후보키
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42890>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob60/60-1.png)
![이미지](/assets/images/Programmers/Lv2/prob60/60-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    
    static String[][] Table;
    static int length;
    static ArrayList<HashSet<Integer>> candidateKey;
        
    public int solution(String[][] relation) {
                
        candidateKey = new ArrayList<HashSet<Integer>>();
        length = relation[0].length;
        Table = relation;
        
        for(int i = 1; i <= relation[0].length; i++){
            makeKey(-1, 0, i, new HashSet<Integer>());
        }
        
        return candidateKey.size();
    }
    
    static void makeKey(int idx, int count, int target, HashSet<Integer> HS){
        if(count == target){
            if(isUnique(HS) == false) return;
            for(HashSet<Integer> key : candidateKey){
                if(HS.containsAll(key)) return;
            }
            candidateKey.add(HS);
            return;
        }
        for(int i = idx + 1; i < length; i++){
            HashSet<Integer> newSet = new HashSet<Integer>(HS);
            newSet.add(i);
            makeKey(i, count + 1, target, newSet);
        }
    }
    
    static boolean isUnique(HashSet<Integer> set){
       ArrayList<String> list = new ArrayList<String>();
    	for(int i = 0; i < Table.length; i++) {
    		String temp = "";
    		
            for(int index : set) {
    			temp+= Table[i][index];
    		}
    		if(!list.contains(temp)) {
    			list.add(temp);
    		}else {
    			return false;
    		}
    	}
    	return true;
    }
}
``` 
  
## 👊🏻 내 전략 
  
1. 후보키가 될만한 조합을 우선 다 구한다.
2. 그 조합이 유니크한지 추려내는데, 각 조합을 String형식으로 표현해서 해당 String이 존재하는지 검색한다.
3. 유니크 하다면, 이미 존재하고 있는 key는 아닌지 확인한다. 
   - 최소성은 containsAll을 활용해서 찾아낸다.
4. 아니라면 추가!
  
끝-!
