---
title: "[클래스의 이해] 12. 다형성(Polymorphism)이란"
excerpt: "Java polymorphism에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - 다형성
  - polymorphism
  - 객체지향
last_modified_at: 2021-03-30 20:00:20
---

# 12. 다형성(Polymorphism)
<span style="color:grey">[자바 : 클래스의 이해와 객체지향 프로그래밍]내용입니다.</span>
  
상속한 클래스의 오브젝트는 슈퍼 클래스로도 서브클래스로도 다룰 수 있습니다.  
이렇게 하나의 오브젝트와 메서드가 많은 형태를 가지고 있는 것을 **다형성**이라고 합니다.  
  
## 12-1. 슈퍼 클래스의 오브젝트 생성
서브 클래스의 오브젝트는 슈퍼 클래스의 오브젝트에 대입할 수 있습니다.  
![이미지](/assets/images/JAVA/polymorphism/poly1.png)
  
하위클래스 객체를 상위클래스 객체에 대입하여 사용할 수 있습니다.  
![이미지](/assets/images/JAVA/polymorphism/poly2.png)
  
하지만, 상위클래스의 객체를 하위클래스의 객체로 대입할 수는 없습니다.
![이미지](/assets/images/JAVA/polymorphism/poly3.png)
  
Sub 클래스의 설계도를 바탕으로 name 변수를 사용하면,  
생성된 객체에는 해당 변수가 없어 에러가 발생하게 된다.  
이러한 이유로 **상위클래스의 객체를 하위클래스의 객체로 대입하는 것을 막고 있습니다.**  

## 12-2. 실습
```java
abstract class Calc{
	int a = 5;
	int b = 6;
	
	abstract void plus();
}

class MyCalc extends Calc{
	void plus()  { System.out.println(a+b);}
	void minus() { System.out.println(a-b);}
}

public class Polymorphism1 {

	public static void main(String[] args) {
		MyCalc mycalc1 = new MyCalc();
		mycalc1.plus();
		mycalc1.minus();
		
		//하위 클래스 객체를 상위 클래스 객체에 대입 
		Calc mycalc2 = new MyCalc();
		mycalc2.plus();
		//다음 메서드는 설계도에 없다. 따라서 사용할 수 없다. 
		//mycalc2.minus(); 하위 클래스 객체에만 있는 메서드이므로! 오류난다. 		
	}
}
```
  
  
끝-!