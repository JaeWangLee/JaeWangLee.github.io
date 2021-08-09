---
title: "[프로그래머스] [LV2] 거리두기 확인하기"
excerpt: "Java로 풀이"
toc: true
toc_sticky: true
categories:
  - Programmers
tags:
  - codingtest
  - algorithm
  - bfs
last_modified_at: 2021-08-09 16:00:20
---

# 📚 거리두기 확인하기
  
링크📎 : <https://programmers.co.kr/learn/courses/30/lessons/81302#>  
  
>난이도 ⭐️⭐️
  
## 📖 문제    
  
![이미지](/assets/images/Programmers/Lv2/prob57/57-1.png)
![이미지](/assets/images/Programmers/Lv2/prob57/57-2.png)
![이미지](/assets/images/Programmers/Lv2/prob57/57-3.png)
![이미지](/assets/images/Programmers/Lv2/prob57/57-4.png)
![이미지](/assets/images/Programmers/Lv2/prob57/57-5.png)
![이미지](/assets/images/Programmers/Lv2/prob57/57-6.png)
  
## 📝 내 풀이  
  
```java  
import java.util.*;

class Solution {
    
    static char[][] map;
    static boolean[][] visited;
    static int[] answer;
    
    static int[] dx = {0,0,1,-1};
    static int[] dy = {1,-1,0,0};
    
    static class Node{
        int x;
        int y;
        
        public Node(int x, int y){
            this.x = x;
            this.y = y;
        }
    }
    
    static Queue<Node> q = new LinkedList<Node>();
    
    public int[] solution(String[][] places) {
        answer = new int[] {1,1,1,1,1};
        map = new char[places.length][5];
        visited = new boolean[places.length][5];
            
        for(int i = 0; i < places.length; i++){
            for(int j = 0; j < places[0].length; j++){
                String temp = places[i][j];
                for(int k = 0; k < 5; k++){
                    map[j][k] = temp.charAt(k);
                }
                Arrays.fill(visited[j], false);
            }
            
            for(int j = 0; j < map.length; j++){
                for(int k = 0; k < 5; k++){
                    if(map[j][k] == 'P') {
                        bfs(map, j, k, i);
                    }
                    if(answer[i] == 0) break;
                }
                if(answer[i] == 0) break;
            }
        }
        return answer;
    }
    
    static void bfs(char[][] map, int x, int y, int idx){
        visited[x][y] = true;
        q.offer(new Node(x,y));
        
        while(!q.isEmpty()){
            if(answer[idx] == 0) break;
            Node now = q.poll();
            if(Math.abs(now.x - x) + Math.abs(now.y - y) > 2) continue;
            
            for(int i = 0; i < 4; i++){
                int nx = now.x + dx[i];
                int ny = now.y + dy[i];
                
                if(Math.abs(nx - x) + Math.abs(ny - y) > 2) continue;
                
                if(0 <= nx && nx < map.length && 0 <= ny && ny < map[0].length){
                    if(visited[nx][ny] == false){
                        visited[nx][ny] = true;
                        if(map[nx][ny] == 'O')
                            q.add(new Node(nx, ny));
                        else if(map[nx][ny] == 'P'){
                            if(Math.abs(nx - x) + Math.abs(ny - y) <= 2){
                                if(map[x][ny] != 'X' || map[nx][y] != 'X'){
                                    answer[idx] = 0;
                                    break;   
                                }
                            }
                        }
                    }
                }
            }
        }
        q.clear();
        
        for(int i = 0; i < 5; i++)
            Arrays.fill(visited[i], false);
    }
}
``` 
  
## 👊🏻 내 전략 
  
1. places의 원소를 꺼내서 map을 만든다.
2. map에서 'P'에 해당하는 경우 BFS를 돌린다.
3. BFS를 돌리더라도 맨해튼 거리를 초과하는 경우는 bfs를 빠져나온다.
  
끝-!
