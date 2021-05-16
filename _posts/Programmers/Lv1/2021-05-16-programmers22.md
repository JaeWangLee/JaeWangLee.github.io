---
title: "[프로그래머스] [LV1] K번째 수"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-16 22:17:20
---

# 📚 K번째 수
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42748>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/22-1.png)
![이미지](/assets/images/Programmers/Lv1/22-2.png)
![이미지](/assets/images/Programmers/Lv1/22-3.png)
![이미지](/assets/images/Programmers/Lv1/22-4.png)

  
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int[] solution(int[] array, int[][] commands) {
        int[] answer = new int[commands.length];
        
        for(int i = 0; i < commands.length; i++){
            answer[i] = cuttingAndFind(array, commands[i]);
        }
        
        return answer;
    }
    
    public int cuttingAndFind(int[] arr, int[] req){
        
        int[] temp = new int[req[1] - req[0] + 1];
        int count = 0;
        
        for(int i = req[0]-1; i < req[1]; i++){
            temp[count++] = arr[i];
        }
        
        Arrays.sort(temp);
            
        return temp[req[2]-1];
    }
}
```
  
끝-!
