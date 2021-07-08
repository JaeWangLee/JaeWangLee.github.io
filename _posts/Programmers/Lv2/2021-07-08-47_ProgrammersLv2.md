---
title: "[프로그래머스] [LV2] 타겟 넘버"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - dfs
last_modified_at: 2021-07-08 11:30:20
---

# 📚 타겟 넘버
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/43165>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob47/47-1.png)

## 📝 내 풀이  
  
```java  
class Solution {
    public int solution(int[] numbers, int target) {
        int answer = 0;
        
        answer = dfs(numbers, target, 0, 0);
        
        return answer;
    }
    
    public int dfs(int[] numbers, int target, int node, int sum){
    
        if(node == numbers.length)
            return (sum == target ? 1 : 0);
        
        return dfs(numbers, target, node + 1, sum + numbers[node])
            + dfs(numbers, target, node + 1, sum - numbers[node]);
    }
}
``` 
  
## 👊🏻 내 전략 
  
각 숫자의 앞마다 부호를 바꿔가면서 체크를 해야한다.  
부호가 위치한 곳을 각 node라고 생각을 했고,  
각 node 마다 2가지의 선택사항이 주어지기 때문에,  
dfs로 풀이를 하게 되었다.  
  
끝-!
