---
title: "[프로그래머스] [LV2] 게임 맵 최단거리"
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
last_modified_at: 2021-07-20 14:00:20
---

# 📚 게임 맵 최단거리
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/1844>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob51/51-1.png)
![이미지](/assets/images/Programmers/Lv2/prob51/51-2.png)
![이미지](/assets/images/Programmers/Lv2/prob51/51-3.png)
![이미지](/assets/images/Programmers/Lv2/prob51/51-4.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    
    static int[] dx = {1,-1,0,0};
    static int[] dy = {0,0,1,-1};
    
    static class Node{
        int x;
        int y;
        
        public Node(int x, int y){
            this.x = x;
            this.y = y;
        }
    }
    
    static Queue<Node> q = new LinkedList<Node>();
    static int[][] visited;
    static int length = 0;
    static int answer = -1;
    
    public int solution(int[][] maps) {
        visited = new int[maps.length][maps[0].length];        
        bfs(maps, 0, 0);
        return answer;
    }
    
    static void bfs(int[][] maps, int x, int y){
        visited[x][y] = 1;
        q.offer(new Node(x,y));
        
        while(!q.isEmpty()){
            Node now = q.poll();
            
            for(int i = 0; i < 4; i++){
                int nx = now.x + dx[i];
                int ny = now.y + dy[i];
                
                if(0 <= nx && nx < maps.length && 0 <= ny && ny < maps[0].length){
                    if(maps[nx][ny] == 1 && visited[nx][ny] == 0){
                        visited[nx][ny] = visited[now.x][now.y] + 1;
                        q.add(new Node(nx, ny));
                        if(nx == maps.length -1 && ny == maps[0].length - 1){
                            answer = visited[nx][ny];
                            q.clear();
                        }
                    }
                }
            }
        }        
    }
}
``` 
  
## 👊🏻 내 전략 
  
1. bfs를 사용한다.
2. visit을 주로 boolean으로 체크해주었는데 여기서는 걸어온 길이(?)를 저장했다.
   - 0이면 방문하지 않은 곳, 길이는 이전 길이 + 1
3. 젤 오른쪽 아래의 좌표가 현재 위치라면 q를 날리고, 그 값을 answer에 저장하여 끝낸다
  
끝-!
