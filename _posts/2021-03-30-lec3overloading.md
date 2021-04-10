---
title: "[강의노트] 오버로딩의 개념"
excerpt: "Java Overloading에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - overloading
  - 객체지향
last_modified_at: 2021-03-30 13:08:20
---

# 3. 오버로딩
<span style="color:grey">[자바 : 클래스의 이해와 객체지향 프로그래밍]내용입니다.</span>
  
**오버로딩**이란?  
**하나의 클래스** 내에 인수의 개수나 형이 다른 **동일한 이름의 메서드를 여러 개 정의하는 것**이다.  
![이미지](/assets/images/JAVA/overloading/overloading1.png)

> 객체지향언어가 아닌 C에서는 오버로딩을 할 수 없다.

## 3-1. 실습

```java

//클래스에 같은 이름의 메서드가 여러 개 존재
class Calc{
	int add(int a, int b) {
		return a+b;
	}
	int add(int a){
		return a+1;
	}
	double add(double a, double b) {
		return a+b;
	}
}

public class Calculation {

	public static void main(String[] args) {		
		Calc calc = new Calc();
		// 메서드 호출
		int nReturn1 = calc.add(3,9);
		int nReturn2 = calc.add(3);
		double nReturn3 = calc.add(3.0,9.0);
		
		System.out.println("Return1 : " + nReturn1);
		System.out.println("Return2 : " + nReturn2);
		System.out.println("Return3 : " + nReturn3);
	}

}
```
> 같은 이름의 메서드를 어떻게 구분하냐면 ? 메서드로 전달하는 인수로 구분!  

![이미지](/assets/images/JAVA/overloading/overloading2.png)
  
  
끝-!😋