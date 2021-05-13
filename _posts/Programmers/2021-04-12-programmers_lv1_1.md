---
title: "[프로그래머스] [LV1] 두 정수 사이의 합"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-04-12 23:11:20
---

# 📚 두 정수 사이의 합
  
>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/1.png)
  
## 📝 내 풀이  
  
```java
class Solution {
    public long solution(int a, int b) {
     
        long sum = 0;
        
        if(a<=b)
            for(int i = a; i <= b; i++)
                sum+=i;
        else if(a>b)
            for(int i = b; i <= a; i++)
                sum+=i;
        
    return sum;
    }
}
```

if문과 for문을 돌리면 풀리는 간단한 문제이나,  
자료형의 크기를 고려하지 못한다면 틀리기 쉽상이다.  
여기서는 sum의 자료형을 long으로 바꾸어 할당하였다.  
integer형의 경우 틀리게 되니 주의하자❗️

## ✔️ 자바 변수와 자료형
  
[비트 수 비교]  
- 1 byte : 8 bits  
- 1 short : 2bytes = 16 bits  
- 1 int : 4bytes = 32 bits  
- 1 long : 16bytes = 64 bits  
  
[표현 범위]  
- byte : -128 ~ 127  
- short : -32,768 ~ 32,767  
- int : -2,147,483,648 ~ 2,147,483,647  
- long : -9223372036854775808 ~ 9223372036854775807  
 
