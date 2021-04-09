---
title: "[강의노트] 오버라이딩(Overriding)이란"
excerpt: "Java Overriding에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - Overriding
  - 객체지향
last_modified_at: 2021-03-30 16:18:20
---

# 8. 오버라이딩(Overriding)
<span style="color:grey">[자바 : 클래스의 이해와 객체지향 프로그래밍]내용입니다.</span>
  
## 8-1. 오버라이딩의 정의 
<u>상속된 메서드와 동일한 이름, 동일한 인수를 갖는 메서드를 정의하여 메서드를 덮어쓰는 것</u>입니다.  
  
> 이때 반환 값의 형도 같아야 합니다.  
  
오버라이드는 하위클래스에서 상속받은 메서드를 단순히 재사용하지 않고, 재정의하여 다른 연산을 하고 싶을 때 사용합니다.  
**즉, 하위 클래스에서 상위클래스의 특정 메서드를 재정의하는 것**을 말합니다.  
  
- 기능의 변경  
- 기능의 추가  
  
✔️오버라이드는 많은 객체 지향 디자인 패턴에 매우 자주 사용되는 기법입니다.  
✔️오버라이드는 추상 클래스와 합쳐져서 객체지향 방법론에서 장점으로 많이 거론되는 <확장성>을 실현하는 데 많은 도움을 주게 되므로, 오버라이드 개념에 대한 이해는 필수적입니다.  

## 8-2. 오버라이딩과 오버로딩 구분

**오버라이딩**
- <u>상속의 관계</u>에서 발생한다.  
- 위(부모)에서 아래(자식)으로 연결된다는 점에서 **라**의 'ㅏ'로 위에서 아래로 긋는다는 이미지 느낌으로 외운다.  
  
**오버로딩**
- 한 클래스 내에서 동일한 이름의 메서드가 여러 개 존재한다.
- 모든 메서드는 수평적인 관계이므로 **로**의 'ㅗ'로 옆으로 긋는다는 이미지의 느낌으로 외운다.  
  
## 8-3. 실습

```java
class Animal{
	String name;
	int age;
	
	void printPet() {
		System.out.println("이름 : " + name);
		System.out.println("나이 : " + age);
	}
}
class Dog extends Animal{
	String variety;

	//함수의 오버라이딩
	void printPet() {
		super.printPet();
		System.out.println("종류 : " + variety);
	}
}
public class Pet {

	public static void main(String[] args) {
		Dog dog = new Dog();
		dog.name ="진돌이";
		dog.age = 5;
		dog.variety ="진돗개";
		dog.printPet();
	}

}
```

![이미지](/assets/images/JAVA/inheritance/inheritance5.png)
  
참고강의 : 인프런 <자바 : 클래스의 이해와 객체지향 프로그래밍> 이재환 강사님
  
끝-!