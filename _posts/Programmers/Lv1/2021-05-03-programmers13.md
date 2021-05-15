---
title: "[프로그래머스] [LV1] 모의고사"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-03 23:11:20
---

# 📚 모의고사
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42840?language=java>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/13-1.png)
![이미지](/assets/images/Programmers/Lv1/13-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int[] solution(int[] answers) {
        List<Integer> count = new ArrayList<Integer>();
        
        int[] answer = {};
        
        int[] student1 = {1,2,3,4,5};
        int[] student2 = {2,1,2,3,2,4,2,5};
        int[] student3 = {3,3,1,1,2,2,4,4,5,5};
        int[] n = {0,0,0};
        int max = 0;
        
        for(int i = 0; i < answers.length; i++){
            if(answers[i] == student1[i%5]) n[0]++;
            if(answers[i] == student2[i%8]) n[1]++;
            if(answers[i] == student3[i%10]) n[2]++;
        }
        
        max = n[0] > n[1] ? (n[0] > n[2] ? n[0] : n[2]) : (n[1] > n[2] ? n[1] : n[2]);
    
	    if (n[0] == max) count.add(1);
        if (n[1] == max) count.add(2);
	    if (n[2] == max) count.add(3); 
        
        answer = new int[count.size()];
        for(int i = 0; i < count.size(); i++){
           answer[i] = count.get(i);
        }
        
        return answer;
    }
}
```
  
## Stream 활용!!
Stream을 활용한 풀이가 있어 가져와봤다.
```java
    ArrayList<Integer> list = new ArrayList<>();
    if(maxScore == score[0]) {list.add(1);}
    if(maxScore == score[1]) {list.add(2);}
    if(maxScore == score[2]) {list.add(3);}
    return list.stream().mapToInt(i->i.intValue()).toArray();
```
  
  
끝-!