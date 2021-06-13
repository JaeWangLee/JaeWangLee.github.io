---
title: "[프로그래머스] [LV2] 2개 이하로 다른 비트"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-13 14:45:20
---

# 📚 2개 이하로 다른 비트
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/77885>  
  
>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob23/23-1.png)
![이미지](/assets/images/Programmers/Lv2/prob23/23-2.png)
  
## 📝 내 풀이  
  
첫번째 풀이  
이렇게 풀면 TC 10, 11에서 시간 초과 난다.  
  
```java  
import java.util.*;
class Solution {
    public long[] solution(long[] numbers) {
        long[] answer = new long[numbers.length];
        
        for(int i = 0; i < numbers.length ; i++)
            answer[i] = functionF(numbers[i]);
        
        return answer;
    }
    
    public long functionF(long n){
        
        if(n%2==0) return n+1;
        
        long num = n+1;
        
        ArrayList<Long> list = new ArrayList<Long>();
        
        while(n!=0){
            list.add(n%2);
            n/=2;    
        }
        
        while(true){
            long count = 0;
            long temp = num;
            for(long i : list){
                if(temp%2 != i) count++;
                temp /= 2;
                if(count>2) break;
            }
            if(temp==0&&count<3) break;
            else if(count<2) break;
            num++;
        }
        return num;
    }
}
```  
  
두번째 풀이 - 비트 연산자 이용  
  
```java
import java.util.*;
class Solution {
    public long[] solution(long[] numbers) {
        long[] answer = new long[numbers.length];
        
        for(int i = 0; i < numbers.length ; i++)
            answer[i] = functionF(numbers[i]);
        
        return answer;
    }
    
    public long functionF(long n){
        long cnt = 0;
        long temp = n;
        if(n%2==0) return n+1;
        else{
            while(true){
                if(temp%2==0) break;
                temp/=2;
                cnt++;
            }
        }
        // n+= (1<<cnt) - (1<< cnt-1); 이건 안된다.
        n += Math.pow(2,cnt) - Math.pow(2,cnt-1);
        return n;
    }
}
```
  
끝-!
