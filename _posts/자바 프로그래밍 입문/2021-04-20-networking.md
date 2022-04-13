---
title: "[자바 입문] 28. 네트워킹"
published: false
excerpt: "Java 네트워킹에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - class
  - 네트워킹
  - 객체지향
  - 인프런
last_modified_at: 2021-04-20 21:30:20
---

# 28. 네트워킹
<span style="color:grey">[자바 프로그래밍 입문 강좌 (renew ver.) - 초보부터 개발자 취업까지!!]내용입니다.</span>
    
## 28-1. 네트워크 데이터 입력 및 출력
  
![이미지](/assets/images/Java_프로그래밍_입문/28강/nw1.png)  
    
## 28-2. 소켓(Socket)
  
![이미지](/assets/images/Java_프로그래밍_입문/28강/nw2.png)  
> socket은 전화기와 같은 역할을 한다고 생각하자.
  
## 28-3. Socket 클래스
  
![이미지](/assets/images/Java_프로그래밍_입문/28강/nw3.png)  
  
```java
package testPjt;

import java.net.ServerSocket;

public class MainClass{

    public static void main(String[] args){

        ServerSocket serverSocket = null;
        Socket socket = null;

        try{
            serverSocket = new ServerSocket(9000); // port 번호
            System.out.println("클라이언트 맞을 준비 완료!!");

            socket = serverSocket.accept();// socket을 반환해줌
            System.out.println("클라이언트 연결!!");
            System.out.println("socket : " + socket);

        } catch(Exception e){
            e.printStackTrace();
        } finally{
            try{
                
                if(socket != null) socket.close();
                if(serverSocket != null) serverSocket.close();

            } catch(Exception e){
                e.printStackTrace();
            }
        }
    }

}
```
  
## 28-4. Client와 Server 소켓(Socket)
  
![이미지](/assets/images/Java_프로그래밍_입문/28강/nw4.png)  
  
```java
package testPjt;

import java.io.IOException;
import java.net.Socket;

public class MainClassSocket{

    public static void main(String[] args){

        Socket socket = null; // 클라이언트는 소켓만 준비하면 됨.

        try{
            socket = new Socket("localhost", 9000);
            System.out.println("서버 연결!!");
            System.out.println("socket : " + socket);

        } catch(IOException e){
            e.printStackTrace();
        } finally{
            try{
                if(socket != null) socket.close();
            } catch(IOException e){
                e.printStackTrace();
            }
        }
    }

}
```

## 28-5.양방향 통신
  
![이미지](/assets/images/Java_프로그래밍_입문/28강/nw5.png)  
  
### 클라이언트 Code
  
```java
package testPjt;

import java.io.DataInputStream;

public class ClientClass{

    public static void main(String[] args){

        Socket socket = null; // 클라이언트는 소켓만 준비하면 됨.

        OutputStream outputStream = null;
        DataOutputStream dataOutputStream = null;

        InputStream inputStream = null;
        DataInputStream dataInputStream = null;

        Scanner scanner = null;

        try{
            socket = new Socket("localhost", 9000);
            System.out.println("서버 연결 완료~~");

            outputStream = socket.getOutputStream();
            dataOutputStream = new DataOutputStream(outputStream);

            inputStream = socket.getInputStream();
            dataInputStream = new DataInputStream(inputStream);

            scanner = new Scanner(System.in);

            while(true){
                System.out.println("메시지 입력~~");
                String outMessage = scanner.nextLine();
                dataOutputStream.writeUTF(outMessage);
                dataOutputStream.flush();

                String inMessage = dataInputStream.readUTF();
                System.out.println("inMessage : " + inMessage());

                if(outMessage.equals("STOP")) break;
            }

        } catch(IOException e){
            e.printStackTrace();
        } finally{
            try{
                if(socket != null) socket.close();
            } catch(IOException e){
                e.printStackTrace();
            }
        }
    }

}
```
  
### 서버 Code
  
```java
package testPjt;

import java.io.DataInputStream;

public class ServerClass{

    public static void main(String[] args){

        ServerSocket serverSocket = null;
        Socket socket = null; 

        InputStream inputStream = null;
        DataInputStream dataInputStream = null;

        OutputStream outputStream = null;
        DataOutputStream dataOutputStream = null;

        try{
            serverSocket = new ServerSocket("localhost");
            System.out.println("클라이언트 맞을 준비 완료");

            socket = serverSocket.accpet();
            System.out.println("클라이언트 연결~~");
            System.out.println("socket : " + socket);

            inputStream = socket.getInputStream();
            dataInputStream = new DataInputStream(inputStream);

            outputStream = socket.getOutputStream();
            dataOutputStream = new DataOutputStream(outputStream);

            while(true){
                String ClientMessage = dataInputStream.readUTF();
                System.out.println("clientMessage : " + clientMessage);

                dataOutputStream.writeUTF("메시지 전송 완료~~");
                dataOutputStream.flus();

                if(ClientMessage.equals("STOP")) break;
            }

        } catch(IOException e){
            e.printStackTrace();
        } finally{
            try{
                if(socket != null) socket.close();
            } catch(IOException e){
                e.printStackTrace();
            }
        }
    }

}
```
  
끝-!😋