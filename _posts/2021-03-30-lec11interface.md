---
title: "[강의노트] 인터페이스(Interface)란"
excerpt: "Java Interface에 대해 알아보자"
toc: true
toc_sticky: true 
categories:
  - Java
tags:
  - java
  - 인터페이스
  - interface
  - 객체지향
last_modified_at: 2021-03-30 17:00:20
---

# 11. 인터페이스
<span style="color:grey">[자바 : 클래스의 이해와 객체지향 프로그래밍]내용입니다.</span>
  
## 11-1. 인터페이스의 정의  
상속 관계가 아닌 클래스에 (추상 클래스처럼) 기능을 제공하는 구조입니다.  
클래스와 비슷한 구조이지만,  
<u>정의</u>와 <u>추상 메서드</u>만이 멤버가 될 수 있다는 점이 다릅니다.  
클래스에서 인터페이스를 이용하도록 하게 하는 것을 **'인터페이스의 구현'** 이라고 하며,  
인터페이스를 구현하기 위해서는 **implements**를 사용합니다.

![이미지](/assets/images/JAVA/interface/interface1.png)
> 인터페이스는 항상 pulic으로 선언되어 있음.  
  
인터페이스의 정의는 다음과 같이 수식자를 생략할 수 있습니다.
![이미지](/assets/images/JAVA/interface/interface2.png)
  
또한 인터페이스는 몇 개라도 구현할 수 있습니다.
![이미지](/assets/images/JAVA/interface/interface3.png)

## 11-2. 인터페이스의 상속
인터페이스도 클래스처럼 상속할 수 있습니다.
![이미지](/assets/images/JAVA/interface/interface4.png)
  
복수의 인터페이스를 상속하여 새로운 인터페이스를 만들 수 있습니다.
![이미지](/assets/images/JAVA/interface/interface5.png)
  
>**extends**와 **implements**  
다른 클래스를 상속하고, 또한 인터페이스를 구현하는 경우에 다음과 같이 정의하며,  
extends를 먼저 기술합니다.  
![이미지](/assets/images/JAVA/interface/interface6.png)

## 11-3. 실습

**첫번째**  
여러 개를 동시에 인터페이스 할 때
```java
package step1;


interface Greet {
	void greet(); // 추상이고 public이다.
}

interface Bye{
	void bye(); // 추상이고 public이다.
}

class Morning implements Greet, Bye {
	//여러 개를 받을 수 있다.
	//상속관계가 아닌데(연관성이 없는) 기능을 넣고자 할때 인터페이스를 주로 사용한다.

	
	public void bye() {
		System.out.println("안녕히 계세요.");
	}

	
	public void greet() {
		System.out.println("안녕하세요.");
	}
	
}

public class Meet {

	public static void main(String[] args) {
		Morning morning = new Morning();
		morning.greet();
		morning.bye();
	}

}

```

**두번째**  
인터페이스가 인터페이스를 상속받았을때.
```java
package step2;

interface Greet {
	void greet(); // 추상이고 public이다.
}

interface Bye extends Greet{
	void bye(); // 추상이고 public이다.
}

class Morning implements Bye {
	
	public void bye() {
		System.out.println("안녕히 계세요.");
	}

	
	public void greet() { //Bye가 Greet를 상속받았기 때문이다.
		System.out.println("안녕하세요.");
	}
	
}


public class Meet {

	public static void main(String[] args) {
		Morning morning = new Morning();
		morning.greet();
		morning.bye();
	}
}

```

  
참고강의 : 인프런 <자바 : 클래스의 이해와 객체지향 프로그래밍> 이재환 강사님
  

끝-!