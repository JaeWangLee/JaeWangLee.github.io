---
title: "[자바 입문] 27. 입력과 출력"
published: false
excerpt: "Java 입력과 출력에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - class
  - 입력과출력
  - 객체지향
  - 인프런
last_modified_at: 2021-04-19 23:00:20
---

# 27. 입력과 출력
<span style="color:grey">[자바 프로그래밍 입문 강좌 (renew ver.) - 초보부터 개발자 취업까지!!]내용입니다.</span>
    
## 27-1. 입/출력 이란?
  
![이미지](/assets/images/Java_프로그래밍_입문/27강/io1.png)  
  
## 27-2. 입/출력 기본 클래스
  
![이미지](/assets/images/Java_프로그래밍_입문/27강/io2.png)  
  
> InputStream과 OutputStream은 추상 클래스임.  
  
## 27-3. FileInputStream/FileOutputStream
  
![이미지](/assets/images/Java_프로그래밍_입문/27강/io3.png)  
  
1byte 단위로 데이터를 읽어들인다.  
  
### read()  
```java
public MainCalss001{
    public static void main(String[] args){

        //read()  
        //네트워크와 관련된 읽기/쓰기는 반드시 예외처리 해야한다.  
        InputStream inputStream = null;
        try{
            inputStream = new FileInputStream("C:\\java\\pjt_ex\\hello.txt");  
            int data = 0;

            while(true){
                try{
                    data = inputStream.read();
                }catch(IOException e){
                    e.printStackTrace();
                }
                if(data == -1) break; // 더이상 읽을 게 없을 때
                System.out.println("data : " + data);
            }
        }catch(FileNotFoundException e){
            e.printStackTrace();
        }finally{
            try{
                if(intputStream != null) inputStream.close();
            }catch(IOException e){
                e.printStackTrace();
            }
        }
    }
}
```

### read(byte[])  
```java
public MainCalss002{
    public static void main(String[] args){

        //read(byte[])  
        InputStream inputStream = null;
        try{
            inputStream = new FileInputStream("C:\\java\\pjt_ex\\hello.txt");  
            int data = 0;
            byte[] bs = new byte[3];//배열의 크기만큼씩 읽어온다.

            while(true){
                try{
                    data = inputStream.read(bs);
                }catch(IOException e){
                    e.printStackTrace();
                }
                if(data == -1) break; // 더이상 읽을 게 없을 때
                System.out.println("data : " + data);
                for(int i = 0; i < bs.length; i++){
                    System.out.println("bs[" + i + "] : "+ bs[i]);
                }
            }
        }catch(FileNotFoundException e){
            e.printStackTrace();
        }finally{
            try{
                if(intputStream != null) inputStream.close();
            }catch(IOException e){
                e.printStackTrace();
            }
        }
    }
}  
  
```
### write()  
```java
public MainCalss003{
    public static void main(String[] args){

        //write()  
        //네트워크와 관련된 읽기/쓰기는 반드시 예외처리 해야한다.  
        OutputStream outputStream = null;
        try{
            outputStream = new FileOutputStream("C:\\java\\pjt_ex\\hello.txt");//이미 존재하면 덮어쓰기, 없으면 생성  
            String data = "Hello java world!";
            byte[] arr = data.getBytes();

            
            try{
                outputStream.write(arr);
            }catch(IOException e){
                e.printStackTrace();
            }
        }catch(FileNotFoundException e){
            e.printStackTrace();
        }finally{
            try{
                if(outputStream != null) outputStream.close();
            }catch(IOException e){
                e.printStackTrace();
            }
        }
}
```

### write(byte[])  
```java
public MainCalss004{
    public static void main(String[] args){

        //write()  
        //네트워크와 관련된 읽기/쓰기는 반드시 예외처리 해야한다.  
        OutputStream outputStream = null;
        try{
            outputStream = new FileOutputStream("C:\\java\\pjt_ex\\hello.txt");//이미 존재하면 덮어쓰기, 없으면 생성  
            String data = "Hello java world!";
            byte[] arr = data.getBytes();

            
            try{
                outputStream.write(arr, 0, 5);//0에서부터 5개만 입력
            }catch(IOException e){
                e.printStackTrace();
            }
        }catch(FileNotFoundException e){
            e.printStackTrace();
        }finally{
            try{
                if(outputStream != null) outputStream.close();
            }catch(IOException e){
                e.printStackTrace();
            }
        }
}
```
## 27-4. 파일 복사 
![이미지](/assets/images/Java_프로그래밍_입문/27강/io4.png)  
```java
public MainCalss005{
    public static void main(String[] args){

        InputStream inputStream = null;
        OutputStream outputStream = null;
        try{
            inputStream = FileInputStream("C:\\java\\pjt_ex\\hello.txt");
            outputStream =  new FileOutputStream("C:\\java\\pjt_ex\\helloCopy.txt");

            byte[] arr = new byte[3];

            while(true){
                int len = inputStream.read(arr);
                if(len = -1) break;
                outputStream.write(arr,0,len);
            }
        }catch(Exception e){
            e.printStackTrace();
        }finally{
            if(inputStream != null){
                try{
                    inputStream.close();
                }catch(Exception e) {e.printStackTrace();}
            }
            if(outputStream != null){
                try{
                    outputStream.close();
                }catch(Exception e) {e.printStackTrace();}
            }
        } 
}
```
## 27-5. DataInputStream, DataOutputStream
![이미지](/assets/images/Java_프로그래밍_입문/27강/io5.png)  
왜냐면 byte단위는 사람이 보기 불편하기 때문에  
  
>문자열 단위로 받아들임.  
  
```java
package lec27Pjt005;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class MainClass {
	
	public static void main(String[] args) {
		
		String fileName = "C:\\java\\pjt\\helloReader.txt";
		
		BufferedReader br = null;
		FileReader fr = null;

		try {

			fr = new FileReader(fileName);
			br = new BufferedReader(fr);

			String strLine;

			while ((strLine = br.readLine()) != null) {
				System.out.println(strLine);
			}

		} catch (IOException e) {
			e.printStackTrace();
		} finally {

			try {
				if (br != null) br.close();
				if (fr != null) fr.close();
			} catch (IOException ex) {
				ex.printStackTrace();
			}

		}

	}

}

```
## 27-6. BufferedReader, BufferedWriter
![이미지](/assets/images/Java_프로그래밍_입문/27강/io6.png)  
```java
package lec27Pjt006;

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class MainClass {
	
	public static void main(String[] args) {
		
		String fileName = "C:\\java\\pjt\\helloWriter.txt";
		
		BufferedWriter bw = null;
		FileWriter fw = null;

		try {

			String str = "Hello Java World~~\n";
			str += "Hello C World~~\n";
			str += "Hello C++ World~~\n";

			fw = new FileWriter(fileName);
			bw = new BufferedWriter(fw);
			bw.write(str);

			System.out.println("end");

		} catch (IOException e) {
			e.printStackTrace();
		} finally {

			try {
				if (bw != null) bw.close();
				if (fw != null) fw.close();
			} catch (IOException ex) {
				ex.printStackTrace();
			}

		}

	}

}

```
끝-!😋