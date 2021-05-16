---
title: "[클래스의 이해] 15. 객체 확인"
excerpt: "Java 객체확인에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - 객체확인
  - 객체지향
last_modified_at: 2021-03-31 19:50:20
---

# 15. 객체 확인
<span style="color:grey">[자바 : 클래스의 이해와 객체지향 프로그래밍]내용입니다.</span>
  
**instanceof**란?  
<u>오브젝트가 지정한 클래스의 오브젝트인지를 조사하기 위한 연산자</u>이다.

![이미지](/assets/images/Java_클래스의_이해/15강/instanceof1.png)
왼쪽 오브젝트가 오른쪽 클래스 또는 서브 클래스의 오브젝트라면 **True** 반환  

## 15-1. 실습

```java

interface Cry{
	void Cry();
}

class Cat implements Cry{

	public void Cry() {
		System.out.println("야옹~");
	}
}

class Dog implements Cry{

	public void Cry() {
		System.out.println("멍멍~");
	}
}

public class CheckCry {
	
	
	public static void main(String[] args) {
		Cry test1 = new Cat();
		if(test1 instanceof Cat) {
			test1.Cry();
		}else {
			System.out.println("고양이가 아닙니다.");
		}
	}

}
```
> 결과는 야옹~  
  
끝-!