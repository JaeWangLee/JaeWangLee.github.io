---
title: "[클래스의 이해] 2. Package의 개념"
published: false
excerpt: "Java Package에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - Package
  - 객체지향
last_modified_at: 2021-03-29 09:08:20
---

# 2. 패키지의 개념
<span style="color:grey">[자바 : 클래스의 이해와 객체지향 프로그래밍]내용입니다.</span>
 
## 2-1. 패키지 선언이 필요한 상황?
  
패키지 선언이 필요한 상황은 다음과 같다.  
  
'B'회사로부터 원(Circle) 개발 의뢰를 받은 'A'회사.  
'A'회사는 team1과 team2가 있었고, 빠른 개발을 위해 일을 분리해 개발시켰다.  
  
team1에게는 <u>원의 넓이</u>를..  
team2에게는 <u>원의 둘레</u>를 구하게 했다.  
...  
그러자 다음 두가지 문제가.. 발생하게 되었다. 🤷‍♂️❌  
  
![이미지](/assets/images/Java_클래스의_이해/2강/class2.png)  
  
### 문제점1. 공간에서의 충돌
**동일 이름의 클래스 파일은 같은 위치에 둘 수 없다.**
✔️ Circle이라는 클래스명을 <u>중복 사용</u>하였다.  
  
### 문제점2. 접근 방법에서의 충돌
**인스턴스 생성 방법에서 두 클래스에 차이가 없다.**
✔️ 클래스가 원의 넓이를 구하는 것인지, 둘레를 구하는 것인지 알 수가 없다.  
  
## 2-2. 패키지 선언
![이미지](/assets/images/Java_클래스의_이해/2강/class4.png)
클래스 생성 시 패키지 이름을 추가하거나, 패키지를 추가 후 하위의 클래스를 생성하거나 선언 방법은 간단하다.  
  
### 공간적, 접근적 충돌 해결을 위한 패키지 선언
  
**클래스 접근 방법의 구분**  
서로 다른 패키지의 두 클래스는 인스턴스 생성 시 사용하는 이름(클래스 변수명)이 다르다.  
  
**클래스의 공간적인 구분**  
서로 다른 패키지의 두 클래스 파일은 저장되는 위치가 다르다.

### 패키지 선언에 따른 문제 해결
![이미지](/assets/images/Java_클래스의_이해/2강/class5.png)

다음과 같이 저장된 클래스의 공간적인 구분이 이루어진다.
![이미지](/assets/images/Java_클래스의_이해/2강/class6.png)

다음과 같이 인스턴스 생성 시 사용하는 이름(클래스명)이 다르다.
![이미지](/assets/images/Java_클래스의_이해/2강/class7.png)

### 패키지 선언 실제 예
![이미지](/assets/images/Java_클래스의_이해/2강/class8.png)
패키지 이름은 모두 **소문자**로 구성하며,  
인터넷 도메인 이름의 역순으로 이름을 구성 (국가.회사.부서)  
  
```java
com.company.area.Circle c1 = new com.company.area.Circle();
com.company.length.Circle c2 = new com.company.length.Circle();
```
  
## 2-3. 실습
  
**team1 package**  
  
```java
package team1;

public class Circle {

	final double PI = 3.14; // final
	double rad;
	
	public void setRad(double r) {
		rad = r;
	}
	//원의 넓이 반환
	public double getArea() {
		return (rad*rad)*PI;
	}

}

```
> 여기서 final이란 상수를 만들어 주기 위함!
  
**team2 package**  
  
```java
package team2;

public class Circle {
	final double PI = 3.14;
	double rad;
	
	public void setRad(double r) {
		rad = r;
	}
	
	// 원의 둘레 반환
	public double getPerimeter() {
		return 2*PI*rad;
	}
}
```
  
**두 개의 패키지를 활용한 예**  
  
```java
public class A_CircleUsing {

	public static void main(String[] args) {
		team1.Circle c1 = new team1.Circle();
		c1.setRad(3.5);
		System.out.println("반지름 3.5 원 넓이 : " + c1.getArea());
		
		team2.Circle c2 = new team2.Circle();
		c2.setRad(3.5);
		System.out.println("반지름 3.5 원 둘레 : " + c2.getPerimeter());
	}
}
```
  
**import를 사용하면 team.을 생략할 수 있다.**  
  
```java
import team1.Circle;

public class B_ImportCircle {

	public static void main(String[] args) {
		Circle c1 = new Circle();
		c1.setRad(3.5);
		System.out.println("반지름 3.5인 원 넓이:" + c1.getArea());
		
		Circle c2 = new Circle();
		c2.setRad(5.5);
		System.out.println("반지름 5.5인 원 넓이:" + c2.getArea());
	}
}
```
  
끝-!