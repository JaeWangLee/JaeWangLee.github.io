---
title: "[프로그래머스] [LV1] 서울에서 김서방 찾기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 22:00:20
---

# 📚 서울에서 김서방 찾기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12919>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/30-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public String solution(String[] seoul) {
        String answer = "";
        int idx = 0;
        
        for(int i = 0; i < seoul.length; i++){
            if(seoul[i].equals("Kim")){
                idx = i;
                break;
            }
        }
        
        answer = "김서방은 " + idx + "에 있다";
        
        return answer;
    }
}
```  
  
## 문자열(String)을 비교하는 방법
### equals를 통한 방법
  
두개의 문자열이 동일한지 비교하는 방법  
`str1.equals(str2)`  
  
### == 을 이용하는 방법  
  
객체가 같은지 비교하는 방법  
문자열을 비교하지는 않습니다.🙅🏻‍♂️  
  
### compareTo()를 이용하는 방법
반환 값에 따라 문자열을 비교할 수 있습니다.  
0 : 두개의 문자열이 동일  
양수 : str2가 str1보다 사전적으로 앞선다는 의미  
음수 : str1이 str2보다 사전적으로 앞선다는 의미  
`str1.compareTo(str2)` 

끝-!
