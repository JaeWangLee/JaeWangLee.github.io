---
title: "[프로그래머스] [LV2] JadenCase 문자열 만들기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-14 22:08:20
---

# 📚 JadenCase 문자열 만들기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12951>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob26/26-1.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public String solution(String s) {
        String answer = "";
        int mode = 1;
        
        for(int i = 0; i < s.length(); i++){
            if(s.charAt(i) == ' '){
                mode = 1;
                answer += s.charAt(i);
            }
            else{
                if(mode == 1){ // 대문자
                    answer += s.substring(i,i+1).toUpperCase();
                    mode = 2;
                }
                else if(mode == 2){ // 소문자
                    answer += s.substring(i,i+1).toLowerCase();
                }
            }
        }
        
        return answer;
    }
}
```
   
끝-!
