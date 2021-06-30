---
title: "[프로그래머스] [LV2] 큰 수 만들기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - stringbuilder
last_modified_at: 2021-06-30 12:05:20
---

# 📚 큰 수 만들기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42883>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob38/38-1.png)

## 📝 내 풀이  
    
```java  
import java.util.*;

class Solution {
    public String solution(String number, int k) {
        String answer = "";
        StringBuilder sb = new StringBuilder();
        
        sb.append(number);
        
        while(k-- > 0){
            int len = sb.length();
            int idx = len-1;
            
            for(int i = 0; i < len - 1; i++){
                if(sb.charAt(i) < sb.charAt(i+1)){
                    idx = i;
                    break;
                }    
            }
            sb.deleteCharAt(idx);
        }
        
        return sb.toString();
    }
}
``` 
   
## 👊🏻 내 전략
  
1. 크기에 가장 영향을 많이 미치는 수 앞에서부터 체크한다.
   - 마치 msb(most significant bit)처럼 말이다.
2. 가장 자릿 수 높은 거부터 차례대로 하면서 뒷자리수의 값이 더 크면 삭제
3. k만큼 반복하면 된다
4. idx의 초기 값을 len-1로 한 이유는 "1000" 1 일때를 생각해보자
   - 만약 idx의 초기 값이 0이라면 "000"이 되어버린다. 
   
  
끝-!
