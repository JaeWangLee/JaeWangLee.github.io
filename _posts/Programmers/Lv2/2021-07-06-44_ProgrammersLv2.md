---
title: "[프로그래머스] [LV2] [1차] 캐시"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-07-06 12:30:20
---

# 📚 [1차] 캐시
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/17680>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob44/44-1.png)
![이미지](/assets/images/Programmers/Lv2/prob44/44-2.png)

## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(int cacheSize, String[] cities) {
        int answer = 0;
        
        if(cacheSize == 0) return cities.length*5;
        
        LinkedList<String> li = new LinkedList<String>();
        
        for(String city : cities){
            city = city.toUpperCase();
            if(li.contains(city)){
                answer += 1;
                li.remove(city);
                li.offer(city);
            }
            else{
                answer += 5;
                if(li.size() == cacheSize)
                    li.poll();
                li.offer(city);
            }
        }
        
        return answer;
    }
}
``` 
  
## 👊🏻 내 전략 
  
LRU 알고리즘? 캐시? 문제를 처음 읽었을 때, 잘 모르는 용어 때문에 이해가 어려웠다.  
하지만, 이해를 하면 아주 간단한 문제인데 설명을 하자면..  
  
cacheSize가 3인 아래의 cities가 있다고 하자.  
["Jeju", "Pangyo", "Seoul", "Pangyo", "Jeju", "Seoul", "Jeju", "Pangyo", "Seoul"]  
첫 시작에는 cache내 아무것도 없기 때문에 제주, 판교, 서울이 cache에 추가된다.  
  
이때, 원래 없던 것을 추가하는 것이기 때문에 `cache miss`가 발생한다.  
그러나 그 다음번에 나오는 판교의 경우 cache내 이미 있으므로 `cache hit`이 발생한다.  
  
여기서 주의할 점은 판교가 hit을 통해 가장 최신의 요소로 변경해줘야 한다는 것이다.  
도식화 하면 아래와 같다.  
  
cache : ["Jeju", "Pangyo", "Seoul"] 에 판교 추가 발생!  
cache는 ["Jeju", "Seoul", **"Pangyo"**]으로 변경됨~  
  
`LinkedList`로 푸는게 좋을 거 같아서 풀었고, 풀이는 상기 코드를 참고 바란다!
  
끝-!
