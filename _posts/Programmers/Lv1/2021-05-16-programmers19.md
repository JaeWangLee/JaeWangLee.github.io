---
title: "[프로그래머스] [LV1] 폰켓몬"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-16 11:33:20
---

# 📚 폰켓몬
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/1845>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/19-1.png)
![이미지](/assets/images/Programmers/Lv1/19-2.png)
![이미지](/assets/images/Programmers/Lv1/19-3.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(int[] nums) {
        int answer = 0;
        ArrayList <Integer> list = new ArrayList<Integer>();
        
        Arrays.sort(nums);
        
        for(int i = 0; i < nums.length; i++)
            if(!list.contains(nums[i]))list.add(nums[i]);
        
        answer = list.size() < nums.length/2 ? list.size() : nums.length/2;
        
        return answer;
    }
}
```
  
배열을 다루는 법을 연습핳 수 있는 문제였다.  
  

끝-!