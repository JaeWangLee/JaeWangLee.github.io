---
title: "[프로그래머스] [LV1] 비밀지도"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-15 19:33:20
---

# 📚 비밀지도
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/17681?language=java>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/18-1.png)
![이미지](/assets/images/Programmers/Lv1/18-2.png)
![이미지](/assets/images/Programmers/Lv1/18-3.png)
![이미지](/assets/images/Programmers/Lv1/18-4.png)
  
## 📝 내 풀이  
  
```java  
class Solution {
    public String[] solution(int n, int[] arr1, int[] arr2) {
        String[] answer = new String[n];
        
        for(int i = 0; i < n; i++)
            answer[i] = compareArr(n, arr1[i], arr2[i]);
        
        return answer;
    }
    
    public String compareArr(int n, int n1, int n2){
        
        String s = "";
        
        String temp1 = Integer.toBinaryString(n1);
        String temp2 = Integer.toBinaryString(n2);
        String s1 = "";
        String s2 = "";
        
        for(int i = 0; i < n - temp1.length(); i++)
            s1 += "0";
        for(int i = 0; i < n - temp2.length(); i++)
            s2 += "0";
        
        s1 += temp1;
        s2 += temp2;
        
        
        for(int i = 0; i < n; i++){
            if(s1.charAt(i) == '1' || s2.charAt(i) == '1') s += "#";
            else s+= " ";
        }   
        
        return s;
    }
}
```
  
간단한 문자열 비교 문제이다.  
문제의 핵심 구현 중 하나는 이진법 변환이며, Integer Class API를 사용했다.  
2진법 변환만 사용하면, 자릿수만큼 0을 채워주지 않는다는 점은 주의하자.  
  
## 이진법 변환(feat. 10진법, 16진법 변환..)
  
Integer Class API를 활용한다.  
to~~String(값)의 형태이며, 다음과 같이 활용할 수 있다. 
  
```java 
int num = 77;
String a2 = Integer.toBinaryString(num);  // 10진수 to 2진수
String a8= Integer.toOctalString(num);    // 10진수 to 8진수
String a16 = Integer.toHexString(num);    // 10진수 to 16진수
 
System.out.println("2 진수 : " + a2);
System.out.println("8 진수 : " + a8);
System.out.println("16 진수 : " + a16);
``` 
  
출력  
```java
2 진수 : 1001101
8 진수 : 115
16 진수 : 4d
```  
  
**반대라면?**  
  
`parseInt` 를 사용한다.  
  
```java
int a2_2 = Integer.parseInt(a2, 2);
int a8_8 = Integer.parseInt(a8, 8);
int a16_16 = Integer.parseInt(a16, 16);
 
System.out.println(a2_2);
System.out.println(a8_8);
System.out.println(a16_16);
```  
  
출력
```java
77
77
77
```  

끝-!