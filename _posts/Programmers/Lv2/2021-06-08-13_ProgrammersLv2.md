---
title: "[프로그래머스] [LV2] 전화번호 목록"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-08 23:00:20
---

# 📚 전화번호 목록
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42577?language=java>  
  
>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob13/13-1.png)
![이미지](/assets/images/Programmers/Lv2/prob13/13-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;
class Solution {
    public boolean solution(String[] phone_book) {
        boolean answer = true;
        
        Arrays.sort(phone_book);
        
        for(int i = 0; i < phone_book.length; i++){
            for(int j = i+1; j < phone_book.length; j++){
                if(phone_book[j].indexOf(phone_book[i])==0) return false;
                else break;
            }
        }
                   
        return answer;
    }
}
```  
  
## 👊🏻 전략  
  
1. 접두어를 정의하자.
   - index가 0인 경우이다.
2. 시간초과가 나니 break;를 적절히 활용하자.
3. break;를 쓰기위해서는 배열이 sorting 되어있어야한다. 
   - 그래야 다음껄 볼 필요없으니까
  
끝-!
