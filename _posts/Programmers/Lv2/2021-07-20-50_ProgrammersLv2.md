---
title: "[프로그래머스] [LV2] 카카오프렌즈 컬러링북"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - queue
  - bfs
last_modified_at: 2021-07-20 12:00:20
---

# 📚 카카오프렌즈 컬러링북
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/1829?language=java>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob50/50-1.png)
![이미지](/assets/images/Programmers/Lv2/prob50/50-2.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    
    static class Node{
        int x;
        int y;
        
        public Node(int _x, int _y){
            this.x = _x;
            this.y = _y;
        }
    }
    
    static int[] dx = {1,-1,0,0};
    static int[] dy = {0,0,1,-1};
    
    static int size = 0;
    static boolean[][] visited ;
    static Queue<Node> q = new LinkedList<Node>();
    
    public int[] solution(int m, int n, int[][] picture) {
        int numberOfArea = 0;
        int maxSizeOfOneArea = 0;

        visited = new boolean[m][n];
        
        for(int i = 0; i < m; i++){
            for (int j = 0; j < n; j++){
                if(picture[i][j] > 0 && visited[i][j] == false){
                    size = 1;
                    numberOfArea++;
                    
                    bfs(m,n,picture,i,j);
                    maxSizeOfOneArea = Math.max(maxSizeOfOneArea, size);
                }
            }
        }
        
        int[] answer = new int[2];
        answer[0] = numberOfArea;
        answer[1] = maxSizeOfOneArea;
        return answer;
    }
    
    static void bfs(int m, int n, int[][] pic, int x, int y){
        visited[x][y] = true;
        q.offer(new Node(x,y));
        
        while(!q.isEmpty()){
            Node now = q.poll();
            
            for(int i = 0; i < 4; i++){                
                int nx = now.x + dx[i];
                int ny = now.y + dy[i];
                
                if(0 <= nx && nx < m && 0 <= ny && ny < n){
                    if(pic[nx][ny] == pic[x][y] && visited[nx][ny] != true){
                        q.add(new Node(nx, ny));
                        visited[nx][ny] = true;
                        size++; 
                    }
                }
            }
            
        }
    }
}
``` 
  
## 👊🏻 내 전략 
  
1. bfs를 사용하면서 size를 체크해나간다.
   - 하나의 bfs를 다 돌고나면 해당 영역의 size가 도출될 것이다.
2. picture 전체를 돌면서, 가지 않은 곳에 대해서 bfs를 반복하면 끝
  
끝-!
