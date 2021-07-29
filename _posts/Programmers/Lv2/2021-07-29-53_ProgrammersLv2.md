---
title: "[프로그래머스] [LV2] 가장 큰 수"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - comparator
last_modified_at: 2021-07-29 20:30:20
---

# 📚 가장 큰 수
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42746#>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob53/53-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public String solution(int[] numbers) {
        String answer = "";
        
        String[] arr = new String[numbers.length];
        
        for(int i = 0; i < numbers.length; i++)
            arr[i] = String.valueOf(numbers[i]);
        
        Arrays.sort(arr, new Comparator<String>(){
            @Override
            public int compare(String s1, String s2){
                return (s2 + s1).compareTo(s1 + s2);
            }
        });
        
        if(arr[0].equals("0")) return "0";
        
        for(String s : arr)
            answer += s;
        
        return answer;
    }
}
``` 
  
## 👊🏻 내 전략 
  
문자열 고려한 정렬을 하면 되는 문제이다.  
  
예를 들어, [3, 30]이 있다 가정하자.  
숫자형으로 정렬을 하게되면 303이 되지만, 문제 요구사항은 330이 되어야 한다.  
이는 a+b와 b+a의 우열을 가리는 문자열 정렬로 해결할 수 있다.
  
1. Comparator을 사용하여 정렬의 기준을 만들자.
   - Comparator는 익명 클래스나 람다식을 사용하여 구현한다.
2. TC를 돌려보면 11번이 오류가 날텐데 이것은 [0,0,0]에 대해 "000"이 되기 때문이다.
   - arr[0]의 원소가 "0"인 경우 "0"을 리턴하도록 한다.  
  
끝-!
