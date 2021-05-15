---
title: "[클래스의 이해] 4. 생성자의 개념"
excerpt: "Java Constructor에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - Constructor
  - 객체지향
last_modified_at: 2021-03-29 13:08:20
---

# 4. 생성자
<span style="color:grey">[자바 : 클래스의 이해와 객체지향 프로그래밍]내용입니다.</span>
  
**생성자**란?  
<u>오브젝트 생성과 함께 자동적으로 호출되는 특수한 메서드</u>를 말합니다.  
  
그러므로 생성자를 기술하지 않는 경우, 인수가 없는 생성자가 자동으로 만들어집니다.  
이것을 **디폴트 생성자**라 합니다.  

![이미지](/assets/images/JAVA/constructor/constructor1.png)

✔️**디폴트 생성자**의 특징 :  
1. 메서드와 같은 모양이지만 반환형이 없다.  
2. 클래스의 이름과 동일한 이름을 갖는다.  

## 4-1. 생성자 오버로딩
생성자는 가장 흔한 오버로딩의 대상입니다.🌝
![이미지](/assets/images/JAVA/constructor/constructor2.png)
  
만약, 개발자가 매개변수가 있는 생성자만 만든 경우, 디폴트 생성자를 호출하면 에러가 발생합니다.  
이 경우 매개변수가 없는 디폴트 생성자를 호출하기 위해서는 디폴트 생성자도 같이 구현해 주어야 합니다.  
![이미지](/assets/images/JAVA/constructor/constructor3.png)

## 4-2. 복제 생성자
동일한 클래스의 오브젝트를 인수로 받아서,  
대응하는 필드에 값을 대입하는 생성자를 **복제 생성자**라고 합니다.  
![이미지](/assets/images/JAVA/constructor/constructor4.png)

## 4-3. 실습
1. Book class의 생성자와 오버로딩

```java
package step1;

class Book{
	String title; // 책 제목
	int price; 	  // 책 가격
	int num; 	  // 주문 수
	
	Book(){
		title = "자바 클래스 기초";
		price = 10000;
	}
	
	Book(String title, int price){
		this.title = title;
		this.price = price;
	}
	
	void print() {
		System.out.println("제      목 : " + title);
		System.out.println("가      격 : " + price);
		System.out.println("주 문 수 량 : " + num);
		System.out.println("합 계 금 액 : " + price*num);
	}
}
public class MyBook {

	public static void main(String[] args) {
		//Book book = new Book();
		Book book = new Book("자바 디자인 패턴", 20000);
		book.num = 5;
		book.print();
	}
}
```
![이미지](/assets/images/JAVA/constructor/constructor5.png)

2. 복제생성자를 이용한 객체 생성
   
```java
package step2;

class Book{
	String title; // 책 제목
	int price;    // 책 가격
	
	Book(String title, int price){
		this.title = title;
		this.price = price;
	}
	
	//복제 생성
	Book(Book copy){
		title = copy.title;
		price = copy.price;
	}
	
	void print() {
		System.out.println("제      목 : " + title);
		System.out.println("가      격 : " + price);
	}
}

public class MyBook {

	public static void main(String[] args) {

		//Book book1 = new Book(); // 디폴트 생성자가 없어서 에러 발
		Book book1 = new Book("자바 클래스 기초", 10000);
		book1.print();
		
		Book book2 = new Book(book1);
		book2.print();
	}

}

```
![이미지](/assets/images/JAVA/constructor/constructor6.png)
  
  
끝-!😋