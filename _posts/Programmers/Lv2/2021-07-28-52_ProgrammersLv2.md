---
title: "[프로그래머스] [LV2] 단체사진 찍기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - 순열
last_modified_at: 2021-07-28 20:00:20
---

# 📚 단체사진 찍기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/1835>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob52/52-1.png)
![이미지](/assets/images/Programmers/Lv2/prob52/52-2.png)
![이미지](/assets/images/Programmers/Lv2/prob52/52-3.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    
    static int count = 0;
    static int[] position = new int[8];
    static boolean[] visited = new boolean[8];
    static HashMap<Character, Integer> kakao;
    
    public int solution(int n, String[] data) {
        int answer = 0;
        count = 0;
        
        kakao = new HashMap<Character,Integer>();
        kakao.put('A', 0);
        kakao.put('C', 1);
        kakao.put('F', 2);
        kakao.put('J', 3);
        kakao.put('M', 4);
        kakao.put('N', 5);
        kakao.put('R', 6);
        kakao.put('T', 7);
        
        Permutation(0, data);
        
        answer = count;
        
        return answer;
    }
    
    public static void Permutation(int idx, String[] data){
        
        if(idx >= 8){
            if(isOK(data))
                count++;
            return;
        }
        
        for(int i = 0; i < 8; i++){
            if(!visited[i]){
                visited[i] = true;
                position[idx] = i;
                Permutation(idx + 1, data);
                visited[i] = false;
            }
        }
    }
    
    public static boolean isOK(String[] data){
        for(int i = 0; i < data.length; i++){
            int x1 = kakao.get(data[i].charAt(0));
            int x2 = kakao.get(data[i].charAt(2));
            char type = data[i].charAt(3);
            int diff = data[i].charAt(4) - '0';
            
            if(type == '='){
                if(Math.abs(position[x1] - position[x2]) != diff + 1) return false;
            }                
            else if(type == '>'){
                if(Math.abs(position[x1] - position[x2]) <= diff + 1) return false;
            }
            else if(type == '<'){
                if(Math.abs(position[x1] - position[x2]) >= diff + 1) return false;
            }
        }            
        return true;
    }
}
``` 
  
## 👊🏻 내 전략 
  
1. 모든 Case를 순열을 이용하여 생성한다.
2. Case가 생성되면 해당 Case가 주어진 조건을 만족하는지 체크하여 count++!
  
끝-!
