---
title: "[프로그래머스] [LV1] 문자열 다루기 기본"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-21 23:33:20
---

# 📚 문자열 다루기 기본
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12918#>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/36-1.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public boolean solution(String s) {
        boolean answer = true;
        
        if(s.length() != 4 && s.length() != 6) answer = false;
        else{
            for(int i = 0; i < s.length(); i++){
                if(s.charAt(i) > '9' || s.charAt(i) < '0'){
                    answer = false;
                    break;
                }
            }
        }
        
        return answer;
    }
}
```  
  
끝-!
