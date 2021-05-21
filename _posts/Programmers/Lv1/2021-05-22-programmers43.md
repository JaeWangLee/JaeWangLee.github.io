---
title: "[프로그래머스] [LV1] 직사각형 별찍기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-05-22 00:02:20
---

# 📚 직사각형 별찍기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/12969?language=java>  

>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/43-1.png)
  
## 📝 내 풀이  
  
```java  
import java.util.Scanner;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int col = sc.nextInt();
        int row = sc.nextInt();

        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++)
                System.out.print("*");
            System.out.println("");
        }
    }
}
```  
  
끝-!
