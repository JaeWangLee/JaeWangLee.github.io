---
title: "[프로그래머스] [LV1] 완주하지 못한 선수"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-02 15:38:20
---

# 📚 완주하지 못한 선수
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/42576>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/8-1.png)

![이미지](/assets/images/Programmers/Lv1/8-2.png)

  
## 📝 내 풀이  
  
첫번째 풀이  
  
```java
import java.util.ArrayList;

class Solution {
    public String solution(String[] participant, String[] completion) {
        
        ArrayList<String> CParr = new ArrayList<String>();
        
        String answer = "";
        
        for(int i = 0; i < completion.length; i++)
            CParr.add(completion[i]);
        
        for(int i = 0; i < participant.length; i++){
            if(!CParr.contains(participant[i])) {
                answer = participant[i];
                break;
                
            }
            CParr.remove(CParr.indexOf(participant[i]));
        }
                          
        return answer;
    }
}
```
  
위의 코드로 풀이 시 정확성 테스트는 통과하지만..  
효율성 테스트에서 통과하지 못한다.  
  
여기서 포인트는 참가자 명단과, 완주한 명단이 주어지고,  
단 한명의 선수만 완주하지 못하였다는 점.  
그렇기 때문에 <u>두 명단을 sorting 후 동일한 Index에서 다른 값이 발견되면,</u>  
완주하지 못한 선수를 찾아낼 수 있다.  
  
두번째 풀이
  
```java
import java.util.*;

class Solution {
    public String solution(String[] participant, String[] completion) {
        
        Arrays.sort(participant);
        Arrays.sort(completion);
        int i;
        
        for(i = 0; i < completion.length; i++){
            if(!participant[i].equals(completion[i])){
                break;
            }
        }
        
        return participant[i];            
    }
}
```  

끝-!