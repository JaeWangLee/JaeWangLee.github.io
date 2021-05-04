---
title: "[프로그래머스] [LV1] 소수 만들기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-03 20:15:20
---

# 📚 소수 만들기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12977>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/10-1.png)

![이미지](/assets/images/Programmers/Lv1/10-2.png)

  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(int[] nums) {
        int answer = 0;
        
        Boolean soSuArr[] = new Boolean[3000];
        Arrays.fill(soSuArr,true);
        soSuArr[0] = false;
        soSuArr[1] = false;
        
        for(int i = 2; i < 3000; i++){
            if(!soSuArr[i]) continue;
            for(int j=2; i*j < 3000; j++){
                soSuArr[i*j] = false;
            }
        }
        
        for(int i = 0; i < nums.length - 2; i++)
            for(int j = i+1; j < nums.length - 1; j++)
                for(int k = j+1; k < nums.length ; k++)
                    if(soSuArr[nums[i]+nums[j]+nums[k]]) answer++;

        return answer;
    }
}
```
  
우선 3개 loop를 돌려야하는데, 돌리면서 소수임을 판별하려면 시간이 초과될거라고 생각이 들었다.  
  
그래서 내 전략은  
1. 우선 1000이하의 자연수 3개이니 3000을 넘지 않을 것이라 판단  
2. 3000 크기의 boolean 배열을 선언하여 미리 소수인지 아닌지 판별해 놓는다.  
3. 그 후 3중 Loop를 돌면서 3개 합이 소수인지 아닌지 확인만 한다.
로 풀었고, 잘 풀렸다.  
  
다른 사람의 풀이를 확인해보니 각자 방식이 천차만별인 것 같다.  
그래서 pass 😅  
  
## ✔️ 배열의 초기화
  
3000 크기의 Boolean 배열을 미리 선언하였고,  
초기 값을 true로 하기 위해  
`Arrays.fill( )` 메서드를 사용하였다.  
  
끝-!