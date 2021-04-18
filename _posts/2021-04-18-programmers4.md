---
title: "[LV1] 두 정수 사이의 합"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-04-18 11:11:20
---

# 📚 두 정수 사이의 합
  
>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/4.png)
  
## 📝 내 풀이  
  
```java
class Solution {
    public long solution(int a, int b) {
        long answer = 0;
        
        if(a < b){   
            for(int i = a; i <= b; i++)
                answer += i;
         }
        else{
            for(int i = b; i <= a; i++)
                answer += i;           
        }
        return answer;
    }
}
```
  
이 문제를 등차수열로 푼 풀이가 재미있어 가져와봤다. 

## 📝 다른 사람 풀이  

```java
class Solution {

    public long solution(int a, int b) {
        return sumAtoB(Math.min(a, b), Math.max(b, a));
    }

    private long sumAtoB(long a, long b) {
        return (b - a + 1) * (a + b) / 2;
    }
}
```