---
title: "[프로그래머스] [LV2] 예상 대진표"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-31 20:17:20
---

# 📚 예상 대진표
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12985>  

>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob4/4-1.png)
![이미지](/assets/images/Programmers/Lv2/prob4/4-2.png)
  
## 📝 내 풀이  
  
```java  
class Solution
{
    public int solution(int n, int a, int b)
    {
        int answer = 0;
        boolean isMeet = false;
        
        // 만난다? 2로 나눴을때 몫이 같다. 
        
        while(!isMeet){
            
            answer++;
            if((a % 2 == 0 && b == a -1) || (b%2==0 && a == b-1) ) isMeet = true;
            else{
                a = a%2 == 0 ? a/2 : a/2 + 1;
                b = b%2 == 0 ? b/2 : b/2 + 1;
            }
        }
        

        return answer;
    }
}
```  
  
끝-!
