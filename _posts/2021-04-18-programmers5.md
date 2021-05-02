---
title: "[프로그래머스] [LV1] 음양 더하기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
last_modified_at: 2021-04-18 11:11:20
---

# 📚 음양 더하기
  
>난이도 ⭐️
  
## 📖 문제  
  
![이미지](/assets/images/Programmers/Lv1/5-1.png)
![이미지](/assets/images/Programmers/Lv1/5-2.png)
  
## 📝 내 풀이  
  
```java
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> absolutes, vector<bool> signs) {
    int answer = 0;
    
    for (int i = 0; i < absolutes.size(); i++){
        if(signs[i]){answer += absolutes[i];}
        else{answer -= absolutes[i];}
    }
    
    return answer;
}
```
  
앞선 풀이에서 말했듯 콜렉션 프레임워크의 경우 크기를 알고자 할 때 .size( )를 사용한다.  
