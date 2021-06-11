---
title: "[프로그래머스] [LV2] 스킬트리"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-11 23:16:20
---

# 📚 스킬트리
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/49993?language=java#>  
  
>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob17/17-1.png)
![이미지](/assets/images/Programmers/Lv2/prob17/17-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(String skill, String[] skill_trees) {
        int answer = 0;
        int pos = 0;
        int skilltree = 0;
        
        for(String s : skill_trees){
            skilltree = 0;
            for(int i = 0; i < s.length(); i++){
                pos = skill.indexOf(s.charAt(i));
                if(skilltree == pos)
                    skilltree++;
                else if(skilltree < pos)
                    break;
                
                if(i == s.length()-1) answer++;
            }   
        }
        
        return answer;
    }
}
```  
   
## 📝 다른 풀이  
  
```java
import java.util.*;

class Solution {
    public int solution(String skill, String[] skill_trees) {
        int answer = 0;
        ArrayList<String> skillTrees = new ArrayList<String>(Arrays.asList(skill_trees));
        //ArrayList<String> skillTrees = new ArrayList<String>();
        Iterator<String> it = skillTrees.iterator();

        while (it.hasNext()) {
            if (skill.indexOf(it.next().replaceAll("[^" + skill + "]", "")) != 0) {
                it.remove();
            }
        }
        answer = skillTrees.size();
        return answer;
    }
}
```
  
Iterator, asList 등에 대해서 알아보자.  
그리고 정규 표현식에 대해서도 알아보자.   
  
끝-!
