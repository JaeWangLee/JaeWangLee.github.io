---
title: "[프로그래머스] [LV2] [1차] 뉴스 클러스터링"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-06-05 23:41:20
---

# 📚 [1차] 뉴스 클러스터링
  
링크📎 : <https://programmers.co.kr/learn/challenges>  

>난이도 ⭐️⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv2/prob7/7-1.png)
![이미지](/assets/images/Programmers/Lv2/prob7/7-2.png)
![이미지](/assets/images/Programmers/Lv2/prob7/7-3.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public int solution(String str1, String str2) {
        int answer = 0;
        answer = (int)(65536 * Jaccard(splitString(str1.toUpperCase()), splitString(str2.toUpperCase())));
        return answer;
    }
    
    public double Jaccard(List<String> a, List<String> b){
        double d = 0.0;
        int dset = 0;
        int union = 0;
        
        if(a.size()==0 && b.size()==0) d = 1.0;
        else{
            for(String s : b){
                for(int i = 0; i < a.size(); i++){
                    if(a.get(i).equals(s)) {
                        a.remove(i);
                        dset++;
                        break;
                    }
                }
            }
            union = b.size() + a.size();
            d = (double)dset/union;
        }
        return d;
    }
    
    public List<String> splitString(String str){
        List<String> temp = new ArrayList<String>();
        
        for(int i = 0; i < str.length()-1; i++){
            if((str.charAt(i)>'Z'||str.charAt(i)<'A')||(str.charAt(i+1)>'Z'||str.charAt(i+1)<'A')) continue;
            temp.add(str.substring(i,i+2));
        }
        
        return temp;
    }
}
```  
  
끝-!
