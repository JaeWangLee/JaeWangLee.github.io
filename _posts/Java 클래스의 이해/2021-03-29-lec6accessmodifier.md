---
title: "[클래스의 이해] 6. 접근제한자란"
excerpt: "Java Access Modifier에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - Access Modifier
  - 접근제한자
  - 객체지향
last_modified_at: 2021-03-29 15:18:20
---

# 6. 접근제한자
<span style="color:grey">[자바 : 클래스의 이해와 객체지향 프로그래밍]내용입니다.</span>
  
## 6-1. 클래스 정의 대상의 public과 default 선언이 갖는 의미
![이미지](/assets/images/Java_클래스의_이해/6강/access1.png)
**public** <u>어디서든 인스턴스 생성이 가능</u>하다.  
**default** <u>동일 패키지로 묶인 클래스 내에서만 인스턴스 생성을 허용</u>한다.  

## 6-2. 인스턴스 멤버 대상의 접근 수준 지시자(접근제한자) 선언
![이미지](/assets/images/Java_클래스의_이해/6강/access2.png)
![이미지](/assets/images/Java_클래스의_이해/6강/access3.png)

## 6-3. 실습
아래와 같이 서로 다른 패키지 2개를 생성하고,  
그 안에 실습할 클래스를 각각 생성합니다.  
  
[package team1]

```java
package team2;
  
public class AnotherClass1 {
	public int num1;
	private int num2;
	protected int num3;
	int num4;
	
	public void test1() {System.out.println("test1");}
	private void test2() {System.out.println("test2");}
	protected void test3() {System.out.println("test3");}
	void test4() {System.out.println("test4");}
	
}
```

[package team2]

```java
package team2;

public class AnotherClass2 {
	public int num1;
	private int num2;
	protected int num3;
	int num4;
	
	public void test1() {System.out.println("test1");}
	private void test2() {System.out.println("test2");}
	protected void test3() {System.out.println("test3");}
	void test4() {System.out.println("test4");}
}

```

패키지는 team1으로 했을때,  
team2 패키지의 경우 

```java

package team1;

import team2.AnotherClass2;

public class AccessTest {
	public int num1;
	private int num2;
	protected int num3;
	int num4;
	
	public void test1() {System.out.println("test1");}
	private void test2() {System.out.println("test2");}
	protected void test3() {System.out.println("test3");}
	void test4() {System.out.println("test4");}
	
	public static void main(String[] args) {
		AccessTest at = new AccessTest(); // 같은 클래스 내부에 있으므로 에러날 게 없음.
		at.num1 = 1;
		at.num2 = 2;
		at.num3 = 3;
		at.num4 = 4;
		
		AnotherClass1 ac1 = new AnotherClass1(); // 같은 패키지 내부이므로 private만 에러
		ac1.num1=1;
		ac1.num2=2; // num2는 private이므로 에러 발생 
		ac1.num3=3;
		ac1.num4=4;
		
		AnotherClass2 ac2 = new AnotherClass2(); // 다른 패키지의 클래이므로 import 해야함
		ac2.num1=1;
		ac2.num2=2; // private이므로 에러 발생 
		ac2.num3=3; // protected 이므로 에러발생 
		ac2.num4=4; // 다른 클래스이므로 에러발생  

	}

}
```
  
  
끝-!😋