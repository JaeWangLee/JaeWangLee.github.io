---
title: "[프로그래머스] [LV1] 두 개 뽑아서 더하기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-04-28 00:11:20
---

# 📚 같은 숫자는 싫어
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/68644>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/7-1.png)

![이미지](/assets/images/Programmers/Lv1/7-2.png)

  
## 📝 내 풀이  
  
```java
import java.util.*;


class Solution {
    public int[] solution(int[] numbers) {
        ArrayList<Integer> num = new ArrayList<Integer>();
        ArrayList<Integer> sum = new ArrayList<Integer>();
        
        for(int i = 0; i < numbers.length; i++)
            num.add(numbers[i]);
        
        for(int i = 0; i < num.size()-1; i++){
            for(int j = i+1; j < num.size(); j++){
            if(!sum.contains(num.get(i)+num.get(j))) 
                sum.add(num.get(i)+num.get(j));
            }    
        }
        
        int[] answer  = new int[sum.size()];
        
        for(int i = 0; i < sum.size(); i++)
            answer[i] = sum.get(i);
            
        Arrays.sort(answer);
        
        return answer;
    }
}
```
  
앞선 풀이에서 말했듯 콜렉션 프레임워크의 경우 크기를 알고자 할 때 .size( )를 사용한다.  
  
우선 반환할 배열의 크기를 알 수 없으므로 ArrayList를 통해 동적으로 값을 받았다.  
  
그 후 ArrayList의 add를 통해 값을 추가하고, .size를 통해 같은 크기의 배열을 만든다.  
  
거기에 값을 대입하고, 출력하면 끝~
  