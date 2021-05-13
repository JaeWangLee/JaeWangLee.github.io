---
title: "[더 자바8] 섹션3. Stream"
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
  - Stream
last_modified_at: 2021-05-03 21:00:20
---

# 섹션3. Stream
<span style="color:grey">[백기선님의 더 자바, Java 8]강의 내용을 정리한 자료입니다.</span>
  
## 3.1. Stream 소개
  
### Stream
sequence of elements supporting sequential and parallel aggregate operations  
- 데이터를 담고 있는 저장소 (컬렉션)이 아니다.
- Funtional in nature, 스트림이 처리하는 <u>데이터 소스를 변경하지 않는다.</u>
  
```java
public static void main(String[] args){
    List<String> names = new ArrayList<>();
    names.add("jaewang");
    names.add("King");
    names.add("toby");
    names.add("foo");

    Stream<String> stringStream = names.stream().map(s->s.toUpperCase);
    //또 다른 stream이 될 뿐, 원본을 (대문자로)변경하는 것이 아님

    names.forEach(System.out::println);
    // jaewang, King, ... 출력됨
}
```
- 스트림으로 처리하는 데이터는 오직 한번만 처리한다.
- 무제한일 수도 있다. (Short Circuit 메소드를 사용해서 제한할 수 있다.)
- 중개 오퍼레이션은 근본적으로 lazy 하다.
  
```java
names.stream().map((s)->{
    System.out.println(s);
    return s.toUpperCase();
});
// 이거 출력 안된다.  
// 중개형 오퍼레이터는 터미널 오퍼레이터가 오기전까지 실행 안된다.  

names.stream().map((s)->{
    System.out.println(s);
    return s.toUpperCase();
}).collect(Collectors.toList());
// 이렇게 종료형 오퍼레이터를 사용하면 출력된다. 
// 그러나. names에서 출력해주는 것이므로 소문자가 출력된다.
// 원본 유지!!

collect.forEach(System.out::println);
//이렇게 하면 대문자로 변경된 collect 스트림을 출력한다. 
```
  
- 손쉽게 병렬 처리할 수 있다.
```java
for(String name : names){
    if(name.startsWith("K")){
        System.out.println(name.toUpperCase());
    }
}
// 이렇게 하면 병렬적으로 루프를 돌기 어렵다.  

List<String> collect = names.parallelStream().map(String::toUpperCase)
            .collect(Collectors.toList());
collect.forEach(System.out::println);
//이런식으로 parallelStream을 사용하면 처리가 쉽다.
```
- 연속된 데이터를 처리하는 오퍼레이션들의 모음이라고 볼 수 있다.  
  
### 스트림 파이프라인
0 또는 다수의 중개 오퍼레이션 (intermediate operation)과 한개의 종료 오퍼레이션 (terminal operation)으로 구성한다.  
스트림의 데이터 소스는 <u>오직 터미널 오퍼네이션을 실행할 때에만 처리한다.</u>  

### 중개 오퍼레이션 (Intermidiate Operation)
**Stream을 리턴한다.**  
Stateless / Stateful 오퍼레이션으로 더 상세하게 구분할 수도 있다.  
(대부분은 Stateless지만 distinct나 sorted 처럼 이전 이전 소스 데이터를 참조해야 하는 오퍼레이션은 Stateful 오퍼레이션이다.)
- filter, map, limit, skip, sorted, ...

### 종료 오퍼레이션 (Terminal Operation)
**Stream을 리턴하지 않는다.**  
- collect, allMatch, count, forEach, min, max, ...

## 3.2. Stream API
  
### 걸러내기
Filter(Predicate)  
- 예) 이름이 3글자 이상인 데이터만 새로운 스트림으로 
  
### 변경하기
Map(Function) 또는 FlatMap(Function)
- 예) 각각의 Post 인스턴스에서 String title만 새로운 스트림으로
- 예) List<Stream<String>>을 String의 스트림으로
  
### 생성하기
generate(Supplier) 또는 Iterate(T seed, UnaryOperator)  
- 예) 10부터 1씩 증가하는 무제한 숫자 스트림
- 예) 랜덤 int 무제한 스트림
  
### 제한하기
limit(long) 또는 skip(long)  
- 예) 최대 5개의 요소가 담긴 스트림을 리턴한다.  
- 예) 앞에서 3개를 뺀 나머지 스트림을 리턴한다.  
  
### 스트림에 있는 데이터가 특정 조건을 만족하는지 확인
anyMatch( ), allMatch( ), nonMatch( )
- 예) k로 시작하는 문자열이 있는지 확인한다. (true 또는 false를 리턴한다.)  
- 예) 스트림에 있는 모든 값이 10보다 작은지 확인한다.  
  
### 개수 세기
count( )
- 예) 10보다 큰 수의 개수를 센다.
  
### 스트림을 데이터 하나로 뭉치기
reduce(identity, BiFunction), collect( ), sum( ), max( )  
- 예) 모든 숫자 합 구하기  
- 예) 모든 데이터를 하나의 List 또는 Set에 옮겨 담기  
  
[코드]  
```java
import java.util.ArrayList;
import java.util.Collection;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;


public class AppRun {

	public static void main(String[] args) {
		List<OnlineClass> springClasses = new ArrayList<>();
		springClasses.add(new OnlineClass(1, "spring boot", true));
		springClasses.add(new OnlineClass(2, "spring data jpa", true));
		springClasses.add(new OnlineClass(3, "spring mvc", false));
		springClasses.add(new OnlineClass(4, "spring core", false));
		springClasses.add(new OnlineClass(5, "rest api developement", false));

		List<OnlineClass> javaClasses = new ArrayList<>();
		javaClasses.add(new OnlineClass(6, "The Java , Test", true));
		javaClasses.add(new OnlineClass(7, "The Java , Code manipulation", true));
		javaClasses.add(new OnlineClass(8, "The Java , 8 to 11", true));

		List<List<OnlineClass>> jaewangEvents = new ArrayList<>();
		jaewangEvents.add(springClasses);
		jaewangEvents.add(javaClasses);

		System.out.println("spring 으로 시작하는 수업");
		springClasses.stream()
				.filter(oc -> oc.getTitle().startsWith("spring"))
				.forEach(oc -> System.out.println(oc.getTitle()));
        
		// 1. filter로 거를껀데. spring으로 시작하는 단어이다. 
		// 2. 개별로 조회해서 프린트 한다.  
		// filter는 중개 오퍼레이터, forEach는 종료 오퍼레이터

		System.out.println("====================================");
		
		System.out.println("close 되지 않은 수업");
		springClasses.stream()
			.filter(oc -> !oc.isClosed()) 
//			.filter(OnlineClass::isClosed) // 클로즈 된거만 필터링
//			.filter(Predicate.not(OnlineClass::isClosed)) // not에 해당 필터
			.forEach(oc -> System.out.println(oc.getTitle()));
		
		System.out.println("====================================");
		System.out.println("수업 이름만 모아서 스트림 만들기");
		springClasses.stream()
//			.map(oc -> oc.getTitle()) // 온라인 클래스의 타이틀만 
			.map(OnlineClass::getTitle) // 메소드 래퍼런스 이용
			// map 에는 oc 타입이 들어온다. 
			.forEach(oc -> System.out.println(oc));
			// forEach에는 String 타입이 들어온다.  
			// 왜냐면 map에서 String 타입으로 변경했으니까!

		System.out.println("====================================");
		System.out.println("두 수업 목록에 들어있는 모든 수업 아이디 출력");
		jaewangEvents.stream()
//			.flatMap(list -> list.stream()) // List를 Stream으로 바꿈
			.flatMap(Collection::stream)    // 그러면 flat 가능하다. 
			.forEach(oc -> System.out.println(oc.getId()));
		
		// 아래는 작성하지 못한다.
		// 그 이유는 jaewangEvents는 2개의 리스트가 합친 것이므로
		// 하나로 만들지 못했기 때문이다.
		// flatMap을 사용하면 각 리스트의 요소들을 꺼내기 때문에
		// 오류가 발생되지 않는다.
		//jaewangEvents.stream()
		//          .forEach(oc -> System.out.println(oc.getId()));
		
		
		System.out.println("====================================");
		System.out.println("10부터 1씩 증가하는 무제한 스트림 중에서 앞에 10개 빼고 최대 10개까지만");
		Stream.iterate(10, i -> i + 1) // 10은 seed에 해당, 무제한 stream이므로 제한해야함.
			.skip(10) // 처음 10개는 skip 하겠음!
			.limit(10) // limit을 정함. 
//			.forEach(i -> System.out.println(i));
			.forEach(System.out::println);
//			21부터 30까지 나옴 ~
		
		System.out.println("====================================");
		System.out.println("자바 수업 중에 Test가 들어있는 수업이 있는지 확인");
		boolean test =  javaClasses.stream().anyMatch(oc -> oc.getTitle().contains("Test"));
		// any : 이중에 어떤거라도 / all : 전부 /// boolean이 리턴됨
		System.out.println(test);
		
		
		System.out.println("====================================");
		System.out.println("스프링 수업 중에 제목이 spring이 들어간 제목을 모아서 List만들기");
		List<String> spring = springClasses.stream() 
			.filter(oc -> oc.getTitle().contains("spring")) // oc가 지나감
			.map(oc -> oc.getTitle()) 
			.collect(Collectors.toList());
		spring.forEach(System.out::println);
	}
}
```
끝-!