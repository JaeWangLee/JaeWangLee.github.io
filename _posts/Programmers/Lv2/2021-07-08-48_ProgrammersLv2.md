---
title: "[프로그래머스] [LV2] 소수 찾기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - dfs
last_modified_at: 2021-07-08 12:30:20
---

# 📚 소수 찾기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42839>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob48/48-1.png)

## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    
    HashSet<Integer> set = new HashSet<Integer>();
    boolean[] chk;
    
    public int solution(String numbers) {
        int answer = 0;
        
        int[] nums = new int[numbers.length()];
        chk = new boolean[numbers.length()];
        
        for(int i = 0; i < numbers.length(); i++)
            nums[i] = numbers.charAt(i) - '0';
        
        Arrays.fill(chk, false);
        
        for(int i = 1; i <= numbers.length(); i++)
            dfs(nums, i, 0, 0);
        
        answer = set.size();
        
        return answer;
    }
    
    public void dfs(int[] nums, int len, int node, int ret){
        if(node == len){
            if(isPrime(ret) == true) set.add(ret);
            return;
        }
        
        for(int i = 0; i < nums.length; i++){
            if(chk[i] == true) continue;
            chk[i] = true;
            
            dfs(nums, len, node+1, ret*10 + nums[i]);
            
            chk[i] = false;
        }
        
    }
    
    public boolean isPrime(int n){
        if(n==0 || n == 1) return false;
        for(int i = 2; i <= Math.sqrt(n); i++) 
			if(n % i == 0) return false;
		return true;
    }
}
``` 
  
## 👊🏻 내 전략 
  
1. 도출된 숫자가 중복될 수 있으니 set으로 관리하자(중복 안되도록)
2. isPrime에서 Math.sqrt()를 사용한 이유는 제곱근을 기준으로 대칭되기 때문이다.
   - 예를들어 16이라고 하면 4를 기준으로 대칭임(1,16), (2,8), (4,4), (8,2), (16,1)
  
끝-!
