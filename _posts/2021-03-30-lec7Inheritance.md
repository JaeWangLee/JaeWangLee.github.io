---
title: "[강의노트] 상속(Inheritance)이란"
excerpt: "Java Inheritance에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - 상속
  - inheritance
  - 객체지향
last_modified_at: 2021-03-30 16:18:20
---

# 7. 상속(Inheritance)
<span style="color:grey">[자바 : 클래스의 이해와 객체지향 프로그래밍]내용입니다.</span>
  
## 7-1. 상속의 정의
**상속**이란?  
<u>상위클래스의 모든 것이 하위클래스에게 전달되는 것</u>을 뜻합니다.  
그러나 상위클래스의 멤버변수와 멤버함수 중 private으로 접근제한이 된 경우에는 하위클래스로 전달이 되지 않습니다.  

![이미지](/assets/images/JAVA/inheritance/inheritance1.png)

## 7-2. 상속의 장점 📝
1. 클래스 간의 체계화된 전체 계층 구조를 파악하기 쉽다.
2. 재사용성 증대 : 기존 클래스에 있는 것을 재사용할 수 있다.
3. 확장 용이 : 새로운 클래스, 데이터, 메서드를 추가하기가 쉽다.
4. 유지보수 용이 : 데이터와 메서드를 변경할 때 상위에 있는 것만 수정하여 전체적으로 일관성을 유지할 수 있다.
![이미지](/assets/images/JAVA/inheritance/inheritance2.png)

## 7-3. 상속의 구현
서브 클래스를 만들기 위해서는 **extends**를 사용합니다.  
그러나 자바에서 여러 개의 클래스를 동시에 상속하는 다중 상속은 허용되지 않습니다.  
![이미지](/assets/images/JAVA/inheritance/inheritance3.png)
  
> 다중 상속을 하려면(?) Interface를 해야한다.  
  
## 7-4. 실습

```java

class Book{
	String title;
	
	void printBook() {
		System.out.println("제  목 : " + title);
	}
}

class Novel extends Book{
	String writer;
	
	void printNov() {
		printBook();
		System.out.println("저  자 : " + writer);
	}
}

class Megazine extends Book{
	int day;
	
	void printMag() {
		printBook();
		System.out.println("발매일 : "+day+"일");
	}
}

public class Bookshelf {

	public static void main(String[] args) {
	Novel nov = new Novel();
	nov.title = "홍길동";
	nov.writer = "허균";
	
	Megazine mag = new Megazine();
	mag.title = "월간 자바";
	mag.day = 20;
	
	nov.printNov();
	System.out.println();
	mag.printMag();

	}

}
```
![이미지](/assets/images/JAVA/inheritance/inheritance4.png)
  
  
끝-!🤨