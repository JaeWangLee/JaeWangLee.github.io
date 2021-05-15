---
title: "[프로그래머스] [LV1] 신규 아이디 추천"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-14 21:15:20
---

# 📚 신규 아이디 추천
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/72410>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/15-1.png)
![이미지](/assets/images/Programmers/Lv1/15-2.png)
![이미지](/assets/images/Programmers/Lv1/15-3.png)
![이미지](/assets/images/Programmers/Lv1/15-4.png)
![이미지](/assets/images/Programmers/Lv1/15-5.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    public String solution(String new_id) {
        
        String answer = "";
        
        answer = processOne(new_id);
        answer = processTwo(answer);
        answer = processThree(answer);
        answer = processFour(answer);
        answer = processFive(answer);
        answer = processSix(answer);
        answer = processSeven(answer);
        
        return answer;
    }
    
    public String processOne(String s){
        s = s.toLowerCase();
        return s;
    }

    public String processTwo(String s){
        
        String temp = "";
        
        for(int i = 0; i < s.length(); i++){
            if((s.charAt(i) >= 'a'&& s.charAt(i)<='z')||(s.charAt(i) >= '0'&& s.charAt(i)<='9')) temp += s.charAt(i);
            else if(s.charAt(i)=='-'||s.charAt(i)=='_'||s.charAt(i)=='.')
                temp += s.charAt(i);
        }
        
        return temp;
    }

    public String processThree(String s){
        
        String temp = "";
        
        for(int i = 0; i < s.length(); i++)
            if(s.charAt(i) == '.'){
                if(i >= 1 && s.charAt(i-1) == '.') continue;
                else temp += s.charAt(i);
            }
            else temp += s.charAt(i);
        
        return temp;
    }

    public String processFour(String s){
        String temp = "";
         
        if(s.charAt(0) != '.') temp += s.charAt(0);
        
        for(int i = 1; i < s.length() - 1; i++) temp += s.charAt(i);
        
        if(s.charAt(s.length()-1) != '.') temp += s.charAt(s.length()-1);
        
        return temp;
    }

    public String processFive(String s){
        if(s.isEmpty()) s += "a";
        return s;
    }

    public String processSix(String s){
        String temp = "";
        if(s.length() >= 16) {
            for(int i = 0; i < 14; i++)
                temp += s.charAt(i);
            if(s.charAt(14) != '.') temp += s.charAt(14); 
        }
        
        else return s;
        
        return temp;
    }

    public String processSeven(String s){
        String temp = "";
        
        if(s.length() <= 2){
            for(int i = 0; i < s.length(); i++){
                temp += s.charAt(i);
            }
            for(int i = s.length(); i < 3; i++){
                temp += s.charAt(s.length()-1);
            }
            
        }
        else return s;
        return temp;
    }
}

```
문제가 너무 길었다..  
요구사항 자체가 어려운 건 없었으나.. 깔끔하게 코드를 짜는 법을 생각해내지 못해  
조금은 부끄러운 코드가 되었다. 
  
replaceAll()을 활용한 다른 풀이가 있어 첨부해둔다.  
## 📝 다른 풀이  
```java
class Solution {
    public String solution(String new_id) {
        String answer = "";
        String temp = new_id.toLowerCase();

        temp = temp.replaceAll("[^-_.a-z0-9]","");
        System.out.println(temp);
        temp = temp.replaceAll("[.]{2,}",".");
        temp = temp.replaceAll("^[.]|[.]$","");
        System.out.println(temp.length());
        if(temp.equals(""))
            temp+="a";
        if(temp.length() >=16){
            temp = temp.substring(0,15);
            temp=temp.replaceAll("^[.]|[.]$","");
        }
        if(temp.length()<=2)
            while(temp.length()<3)
                temp+=temp.charAt(temp.length()-1);

        answer=temp;
        return answer;
    }
}
```
끝-!