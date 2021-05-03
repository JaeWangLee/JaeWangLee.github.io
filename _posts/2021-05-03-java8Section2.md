---
title: "[더 자바8] 섹션2. 인터페이스의 변화"
excerpt: "백기선의 더 자바, Java8 내용입니다."
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - 백기선
  - Java8
  - Interface
  - IntelliJ
  - interface
last_modified_at: 2021-05-03 19:00:20
---

# 섹션2. 인터페이스의 변화
<span style="color:grey">[백기선님의 더 자바, Java 8]강의 내용을 정리한 자료입니다.</span>
  
## 2.1. 인터페이스 기본 메소드와 스태틱 메소드
  
인터페이스의 메소드는 기본적으로 추상 메소드를 사용한다.  
자바8에서는 default 메소드, static 메소드가 추가되었다.  
<span style="color:grey">자바9에서는 private 메소드가 추가되었다.</span>

### 기본 메소드 (Default Methods)  
  
- 인터페이스에 메소드 선언이 아니라 <u>구현체를 제공하는 방법</u>  
- 해당 인터페이스를 구현한 클래스를 깨트리지 않고 새 기능을 추가할 수 있다.  
  > 만약 추상 메소드를 추가하면, 구현해야하기 때문에..  
- 기본 메소드는 구현체가 모르게 추가된 기능으로 그만큼 리스크가 있다.
  - 컴파일 에러는 아니지만 구현체에 따라 런타임 에러가 발생할 수 있다.
  - 반드시 문서화 할 것. (@implSpec 자바독 태그 사용)
  
  > ```java
  > /*
  > @implSpec This implementation returns {@code this.size() == 0}.
  > */
  > ```

- Object가 제공하는 기능 (equals, hasCode)는 기본 메소드로 제공할 수 없다.
  - 구현체가 재정의해야 한다.
- 본인이 수정할 수 있는 인터페이스에만 기본 메소드를 제공할 수 있다.
- 인터페이스를 상속받는 인터페이스에서 다시 추상 메소드로 변경할 수 있다.
- 인터페이스 구현체가 **재정의(Override)** 할 수도 있다.

### 스태틱 메소드
- 해당 타입 관련 헬퍼 또는 유틸리티 메소드를 제공할 때 인터페이스에 스태틱 메소드를 제공할 수 있다.

참고
●	https://docs.oracle.com/javase/tutorial/java/IandI/nogrow.html
●	https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html


## 2.2. 자바 8 API의 기본 메소드와 스태틱 메소드
  
자바 8에서 추가한 기본 메소드로 인한 API 변화
  
다음과 같이 List를 먼저 만들자.  
  
```java
List<String> fruits = new ArrayList<>();
fruits.add("apple");
fruits.add("banana");
fruits.add("grape");
fruits.add("melon");
```
  
### 2.2.1. Iterable의 기본 메소드
  
#### forEach()
다음의 for문을 
```java
for(String fruit : fruits){
    System.out.println(n);
}
```
forEach( )를 사용하여 표현할 수 있다. 
```java
fruits.forEach(fruit -> System.out.println(fruit));
```
즉, <u>내부의 this를 순회하면서 값을 하나씩 받아온다.</u>  
  
이를 메소드 참조(객체 래퍼런스::인스턴스 메소드) 방식으로 표현할 수 있다.  
```java
fruits.forEach(System.out::println);
```
  
[출력]  
```java
apple
banana
grape
melon
```
  
#### spliterator()
자바8부터 추가된 기능  
```java
Spliterator<String> spliterator = fruits.spliterator();
while(spliterator.tryAdvance(System.out::println));
```
  
tryAdvance를 필수로 구현해야하는 Interface이다.  
요소가 남아있으면 계속 수행하며, 있으면 true, 없으면 false를 반환한다.  
  
forEach와 동일한 내용을 출력합니다.  
  
```java
Spliterator<String> spliterator = fruits.spliterator();
Spliterator<String> spliterator1 = spliterator.trySplit();
while(spliterator.tryAdvance(System.out::println));
System.out.println("============");
while(spliterator1.tryAdvance(System.out::println));
```
  
> trySplit( )은 반으로 쪼개는 역할을 한다.  
> parallel하게 쓸 때 유용하다.  
  
[출력]  
```java  
grape
melon
============
apple
banana
```
  
### 2.2.2. Collection의 기본 메소드
#### stream()
  
```java
default Stream<E> stream() {
    return StreamSupport.stream(spliterator(), false);
}
```  
  
Stream은 위와 같이 Spliterator로 만들어져 있다.  
StreamSupport 클래스에 있는 stream static 메서드를 반환한다.  
  
다음과 같이 여러 개의 메소드를 functional하게 이어서 쓸 수 있다.  
```java
long e = fruits.stream().map(String::toUpperCase)
                .filter(s -> s.endsWith("E"))
                .count();
System.out.println(e);
```
  
> fruits을 대문자로 바꾸고, E로 끝나는 단어를, 카운트한다.  
> 결과는 2  
  
#### parallelStream()
  
```java
fruits.forEach(System.out::prinln);
System.out.println("==========");
fruits.parallelStream().forEach(System.out::println);
```
  
[출력] 
```java
apple
banana
grape
melon
=============
grape
melon
banana
apple
```
  
순서를 보장받지는 못한다. 주의하자.  
  
#### removeIf(Predicate)
  
해당 컬렉션의 주어진 조건을 만족하는 모든 요소를 제거한다.  
리턴 형식은 `boolean`
  
```java
fruits.removeIf(s->s.startsWith("a"));
fruits.forEach(System.out::println);
```
  
>apple이 제거된다!  
  
### 2.2.3. Comparator의 기본 메소드 및 스태틱 메소드
#### reversed()
  
역순으로 정렬된 comparator를 반환한다.  
  
```java  
Comparator<String> compareToIgnoreCase = String::compareToIgnoreCase;
fruits.sort(compareToIgnoreCase.reversed());
fruits.forEach(System.out::println);
```
  
> **compareToIgnoreCase**  
> 대소문자를 무시하고 사전적으로 두 문자열을 비교한다.  
  
[출력]
```java
melon
grape
banana
apple
```
  
#### thenComparing()
#### static reverseOrder() / naturalOrder()
#### static nullsFirst() / nullsLast()
#### static comparing()

 
끝-!