---
title: "[클래스의 이해] 10. 추상클래스(abstract class)란"
excerpt: "Java abstract class에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - 추상클래스
  - abstract class
  - 객체지향
last_modified_at: 2021-03-30 19:00:20
---
# 10. 추상 클래스
<span style="color:grey">[자바 : 클래스의 이해와 객체지향 프로그래밍]내용입니다.</span>
  
**추상 메서드**란 <u>처리 내용을 기술하지 않고, 호출하는 방법만을 정의한 메서드</u>를 말하며,  
**추상 클래스**는 이러한 추상 메서드를 한 개라도 가진 클래스를 말합니다.  
  
추상 메서드, 추상 클래스는 **abstract**라는 수식자를 사용하여 다음과 같이 정의합니다.  
![이미지](/assets/images/JAVA/abstractclass/aclass1.png)
> void cry( )는 처리내용을 기술하지 않았다.  
> 즉, <u>추상 메서드</u>이다. 

## 10-1. 추상 클래스와 오버라이딩
추상클래스를 상속하는 클래스는 추상메서드를 반드시 **오버라이딩**해서 구현해야한다.
![이미지](/assets/images/JAVA/abstractclass/aclass2.png)
> 왜냐하면, 처리내용이 기술되어 있지 않으니까~
  
## 10-2. 추상 클래스의 이해와 사용
집(원룸)을 만들기 위한 추상 클래스가 있다면..
![이미지](/assets/images/JAVA/abstractclass/aclass3.png)

## 10-3. 실습

```java

abstract class Animal{
	int age;
	abstract void cry();
}

class Dog extends Animal{

	void cry() {
		System.out.println("멍멍");
	}
}

class Cat extends Animal{

	void cry() {
		System.out.println("야옹");
	}
}



public class AbstractClassEx {

	public static void main(String[] args) {
	Dog dog = new Dog();
	dog.cry();
	
	Cat cat = new Cat();
	cat.cry();
	}

}
```
> 결과는 멍멍/야옹
  
끝-!