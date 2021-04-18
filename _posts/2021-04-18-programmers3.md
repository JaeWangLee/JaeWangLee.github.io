---
title: "[LV1] 내적"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - length
last_modified_at: 2021-04-18 11:11:20
---

# 📚 내적
  
>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/3-1.png)
![이미지](/assets/images/Programmers/Lv1/3-2.png)
  
## 📝 내 풀이  
  
```java
class Solution {
    public int solution(int[] a, int[] b) {
        int answer = 0;
        
        for(int i = 0 ; i < a.length; i++){
            answer += a[i]*b[i];
        }
        
        return answer;
    }
}
```

너무 쉬운문제이다..  
이 문제는 **.length**, **.length( )**, **.size( )** 에 대해 짚고 넘어가는 시간을 갖겠다.  

## ✔️ .length
- 상수  
- 배열의 길이를 알고자 할 때
  
## ✔️ .length( )
- 메소드  
- 문자열의 길이를 알고자 할 때
  
## ✔️ .size( )
- 메서드
- 컬렉션 프레임워크 타입의 길이를 알고자 할 때

> **[컬렉션 프레임워크]**  
> 크기가 고정적인 배열의 문제점을 해결하기 위해 등장한 것으로,  
> Java에서는 <u>List, Set, Map</u> 등이 있다.  
> 이들은 추가, 삭제, 검색, 저장 등이 용이한 특징이 있다.  

