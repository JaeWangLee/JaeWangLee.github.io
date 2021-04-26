---
title: "[LV1] 같은 숫자는 싫어"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-04-27 11:11:20
---

# 📚 같은 숫자는 싫어
  
링크📎 : https://programmers.co.kr/learn/courses/30/lessons/12906#  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/6-1.png)

  
## 📝 내 풀이  
  
```java
import java.util.*;

public class Solution {
    public int[] solution(int []arr) {
        
        ArrayList<Integer> ans = new ArrayList<Integer>();
        
        ans.add(arr[0]);
        
        for(int i = 1; i < arr.length; i++){
            if(arr[i]!=arr[i-1]) ans.add(arr[i]);
        }
        
        int [] answer = new int[ans.size()];
        
        
        for(int i = 0; i < ans.size(); i++)
            answer[i] = ans.get(i);

        return answer;
    }
}
```
  
앞선 풀이에서 말했듯 콜렉션 프레임워크의 경우 크기를 알고자 할 때 .size( )를 사용한다.  
  
우선 반환할 배열의 크기를 알 수 없으므로 ArrayList를 통해 동적으로 값을 받았다.  
  
그 후 ArrayList의 add를 통해 값을 추가하고, .size를 통해 같은 크기의 배열을 만든다.  
  
거기에 값을 대입하고, 출력하면 끝~
  