---
title: "[LV1] 2016년"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-04-18 23:11:20
---

# 📚 2016년
  
>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/2.png)
  
## 📝 내 풀이  
  
```java
class Solution {
    public String solution(int a, int b) {
        String answer = "";
        
        int sum = 0;
        String Day[] = {"FRI", "SAT", "SUN", "MON", "TUE", "WED", "THU"};
        int Month[] = {31,29,31,30,31,30,31,31,30,31,30,31};
        
        for(int i = 1; i < a; i++)
            sum += Month[i-1];
        
        answer = Day[(sum+b-1)%7];
        
        return answer;
    }
}
```

배열과 반복문을 사용해서 쉽게 답을 찾을 수 있는 문제이다.  
  
다만 주의해야할 점은 배열의 [ ]안에 값이 음수가 되지 않도록 연산하도록 하자.  
런타임 에러가 발생할 수 있다.  