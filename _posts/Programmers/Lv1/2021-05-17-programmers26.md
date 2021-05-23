---
title: "[프로그래머스] [LV1] 실패율"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-17 21:30:20
---

# 📚 실패율
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42889>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/26-1.png)
![이미지](/assets/images/Programmers/Lv1/26-2.png)
![이미지](/assets/images/Programmers/Lv1/26-3.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int[] solution(int N, int[] stages) {
        int[] answer = new int[N];
        
        // 도달한 플레이어의 수 = 그 위의 스테이지 클리어한 사람 합(재귀)
        // 아직 클리어하지 못한 수 = stages[i] 합
        int[] ingPerson = new int[N+2];
        int[] clearPerson = new int[N+1];
        HashMap<Integer, Double> failureRate = new HashMap<Integer, Double>();

        Arrays.fill(ingPerson, 0);
        Arrays.fill(clearPerson, 0);
        
        for(int i : stages)
            ingPerson[i]++;
            
        clearPerson[N] = ingPerson[N] + ingPerson[N+1];
        
        for(int i = N-1; i >= 0; i--)
            clearPerson[i] = clearPerson[i+1] + ingPerson[i];
        
        for(int i = 1; i <= N; i++){
            if(clearPerson[i] == 0) failureRate.put(i, 0.0);
            else failureRate.put(i,(double)ingPerson[i]/clearPerson[i]);
        }
        
        ArrayList<Map.Entry<Integer,Double>> al3 = new ArrayList<>(failureRate.entrySet());
        
        Collections.sort(al3, new Comparator<Map.Entry<Integer,Double>>(){
			@Override
			public int compare(Map.Entry<Integer, Double> a, Map.Entry<Integer, Double> b) {
				return b.getValue().compareTo(a.getValue());
			}
		});
        
        int count = 0;
        
        for(Map.Entry<Integer, Double> en:al3)
			answer[count++] = en.getKey();

        return answer;
    }
}
```   
  
와 이건 왜 1 레벨인지 모르겠다..  
Map과 Map 정렬에 대해서는 잘 몰라 구글링하여 활용했다😭  
나중에 한번 정리하도록 하겠다.  
  
  
끝-!
