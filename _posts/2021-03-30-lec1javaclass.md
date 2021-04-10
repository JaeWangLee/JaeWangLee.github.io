---
title: "[강의노트] Class의 개념"
excerpt: "Java Class에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - class
  - object
  - 객체지향
  - 인프런
last_modified_at: 2021-03-30 08:08:20
---

# 1. 클래스의 개념
<span style="color:grey">[자바 : 클래스의 이해와 객체지향 프로그래밍]내용입니다.</span>

## 1-1. 객체의 정의

**객체(Object)** 의 사전적 정의는? '실제로 존재하는 것'입니다.  
  
객체지향프로그래밍(OOP)이나 설계에서는 데이터(실체)와 그 데이터에 관련되는 동작(절차, 방법, 기능)을 모두 포함한 개념을 말합니다. 
  
즉, ✉️ '편지'라는 눈에 보이고 만져지는 물리적인 '객체'도 존재하고,  
📩 '편지를 보낸다'라는 보이지도, 만질수도 없는 개념적인 '객체'도 존재하는 것입니다.  

> 세상 모든 것이 <u>객체</u> 라는 것입니다. 🤦‍♂️

## 1-2. 클래스의 정의
**클래스**란 <U>객체와 관련된 데이터와 처리동작을 한데 모은 것</U>을 말합니다.  
```java
public class Car{
    int fuel;
    int velocity;

    void doAccel(){
        System.out.println("달려~");
    }
    void doBreak(){
        System.out.println("멈춰~");
    }
}
```
예를 들면 **'차'** 라는 객체를 만들고 싶습니다.  
차는 연료나 속도도 존재하고,  
엑셀을 밟거나, 브레이크를 밟는 행위도 존재합니다.  
'차'라는 객체를 만들기 위해 그것에 필요한 것들을 한데 모은 것  
그것이 **'Car'** 라는 클래스이며, 객체를 만들기 위한 **틀** 입니다.

> 클래스는 뭐다? 객체를 만들기위한 '틀'이다.

## 1-3. 클래스의 구조
클래스를 기술하는 것을 **클래스를 정의한다**라고 합니다.  
클래스명 : Npc  
멤버 : 필드(멤버변수) + 메서드  
  
```java
// 클래스 정의
class Npc{
    // 필드 - 데이터
    String name;
    int hp;
    // 메서드 - 동작
    void say(){
        System.out.println("안녕하세요.");
    }
}
```

## 1-4. 객체와 클래스
클래스는 객체의 설계도와 같아, 그 자체만으로는 이용할 수 없습니다.  
설계도를 기초로 실체를 만들 필요가 있으며,  
<u>클래스가 실체화 된 것을 오브젝트(객체)</u>라고 하며,  
실체화하는 작업을 **"오브젝트를 생성한다"** 또는 **"인스턴스화 한다"** 라고 합니다.  

![이미지](/assets/images/JAVA/java_class/class1.png)

실제로 코드상에서 클래스를 객체로 만드는 과정은 다음과 같습니다.  

```java
Book myBook = new Book();
```

## 1-5. 실습

1. 일반적인 클래스의 정의와 객체의 생성
   
```java
// 클래스 정의
class Npc{
	// 필드 - 데이
	String name;
	int hp;
	// 메서드 - 동
	void say() {
		System.out.println("안녕하세요");
	}
}

public class MyNpc {

	public static void main(String[] args) {
		Npc saram1 = new Npc();
		
		saram1.name = "경비";
		saram1.hp = 100;
		System.out.println(saram1.name + ":" +saram1.hp);
		saram1.say();
	}
}
```

> System.out.println은 syso 입력 후 control + space로 자동 완성 가능하다.  
> Mac의 경우 이미 control + space 에 기능이 할당되어 있기 때문에 설정 변경이 필요하다.  
> Eclipse - General - key에서 content assist에서 변경 가능하다.  

2. 메서드만 있는 클래스  
  
```java
//클래스에 메서드만 있는 경우
class Calc{
	int add(int a, int b) {
		return a+b;
	}
}
public class Calculation {
	public static void main(String[] args) {
		// 객체 생성
		Calc calc = new Calc();
		// 메서드 호출
		int nReturn = calc.add(3, 9);
		
		System.out.println("3 + 9 = " + nReturn);
	}
}
```
  
3. 필드만 있는 클래스  
  
```java
//클래스에 필드만 있는 경우
class Book{
	String title;
	String author;
	int price;
}

public class myBook {

	public static void main(String[] args) {
		//객체 생성
		Book book = new Book();
		//필드 접근
		book.title = "클래스의 기초";
		book.author = "홍길동";
		book.price = 10000;
		
		System.out.println(book.title + ":" + book.author + ":" + book.price);
	}

}
```
  
끝-!😋