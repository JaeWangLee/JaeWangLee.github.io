---
title: "[프로그래머스] [LV2] 수식 최대화"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - permutation
last_modified_at: 2021-07-30 21:30:20
---

# 📚 수식 최대화
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/67257>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob55/55-1.png)
![이미지](/assets/images/Programmers/Lv2/prob55/55-2.png)
![이미지](/assets/images/Programmers/Lv2/prob55/55-3.png)
![이미지](/assets/images/Programmers/Lv2/prob55/55-4.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    
    static long max;
    static HashMap<Integer, String> map;
    static int[] position = new int[3];
    static boolean[] visited = new boolean[3];
    static ArrayList<Long> num = new ArrayList<Long>();
    static ArrayList<String> sign = new ArrayList<String>();
    
    public long solution(String expression) {
        long answer = 0;
        
        max = 0;
        
        map = new HashMap<Integer, String>();
        map.put(0,"*");
        map.put(1,"+");
        map.put(2,"-");
        
        String[] arrNum = expression.split("[^0-9]");
        String[] arrSign = expression.split("[0-9]+");
        
        for(String s : arrNum)
            num.add(Long.valueOf(s));
            
        for(int i = 1; i < arrSign.length; i++)
            sign.add(arrSign[i]);
        
        Permutation(0);       
        
        answer = max;
        
        return answer;
    }
    
    public static void Permutation(int idx){
        if(idx > 2){
            makeNum();
            return;
        }
        
        for(int i = 0; i < 3; i++){
            if(!visited[i]){
                visited[i] = true;
                position[idx] = i;
                Permutation(idx + 1);
                visited[i] = false;
            }
        }
    }
    
    public static void makeNum(){
        ArrayList<Long> numCopy = new ArrayList<Long>();
        numCopy.addAll(num);

        ArrayList<String> signCopy = new ArrayList<String>();
        signCopy.addAll(sign);
        
        for(int i = 0; i < 3; i++){
            String temp = map.get(position[i]);
            long tempL;
            for(int j = 0; j < signCopy.size(); j++){
                if(temp.equals(signCopy.get(j))){
                    tempL = 0L;
                    switch(temp){
                        case "+" :
                            tempL = numCopy.get(j) + numCopy.get(j+1);
                            break;
                        case "-" :
                            tempL = numCopy.get(j) - numCopy.get(j+1);
                            break;
                        case "*" :
                            tempL = numCopy.get(j) * numCopy.get(j+1);
                    }
                    numCopy.remove(j+1);
                    numCopy.remove(j);
                    signCopy.remove(j);
                    
                    numCopy.add(j,tempL);
                    
                    j--;
                }
            }
        }
        max = Math.max(max, Math.abs(numCopy.get(0)));
        return ;
    }
}
``` 
  
## 👊🏻 내 전략 
  
1. number와 부호를 split하여 arraylist에 저장한다.
2. 순열을 사용해서 발생할 수 있는 케이스를 생성한다. 
3. 각 케이스에서 ArrayList를 복사하여 case의 우선순위에 따라 계산한다.
  
끝-!
