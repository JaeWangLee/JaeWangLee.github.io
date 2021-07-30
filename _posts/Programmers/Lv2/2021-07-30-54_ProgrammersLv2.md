---
title: "[프로그래머스] [LV2] [3차] 파일명 정렬"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - comparator
last_modified_at: 2021-07-30 20:30:20
---

# 📚 [3차] 파일명 정렬
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/17686>  
  
이전 가장 큰 수 문제와 동일하게 `Comparator`를 사용해서 풀이를 해야한다.  

>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob54/54-1.png)
![이미지](/assets/images/Programmers/Lv2/prob54/54-2.png)
![이미지](/assets/images/Programmers/Lv2/prob54/54-3.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public String[] solution(String[] files) {
        String[] answer = new String[files.length];
        
        Arrays.sort(files, new Comparator<String>(){
            @Override
            public int compare(String s1, String s2){
                
                String head1 = s1.split("[0-9]")[0];
                String head2 = s2.split("[0-9]")[0];
                
                int result = head1.toLowerCase().compareTo(head2.toLowerCase());
                
                if(result == 0) // head가 같을 경우
                {
                    s1 = s1.substring(head1.length());
                    s2 = s2.substring(head2.length());
                    
                    int number1 = Integer.valueOf(s1.toLowerCase().split("[^0-9]")[0]);
                    int number2 = Integer.valueOf(s2.toLowerCase().split("[^0-9]")[0]);
                    
                    result = number1 - number2;
                }
                
                return result;
            }
        });
        
        for(int i = 0; i < files.length; i++)
            answer[i] = files[i];
        
        return answer;
    }
}
``` 
  
## 👊🏻 내 전략 
  
이 문제의 키포인트는 **정규식**과 **Comparator**이다.
  
1. 정규식을 이용하여 적절하게 `HEAD`와 `NUMBER`를 구분할 수 있어야 하며,  
2. Comparator를 이용하여 정렬을 해주면 끝이다. 
  
끝-!
