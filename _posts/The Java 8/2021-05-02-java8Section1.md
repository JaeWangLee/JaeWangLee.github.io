---
title: "[더 자바8] 섹션1. 함수형 인터페이스와 람다"
published: false
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
  - Lambda
last_modified_at: 2021-05-02 12:11:20
---

# 섹션1. 함수형 인터페이스와 람다.
<span style="color:grey">[백기선님의 더 자바, Java 8]강의 내용을 정리한 자료입니다.</span>

## 1.1. 함수형 인터페이스와 람다 표현식 소개
  
### 함수형 인터페이스 (Functional Interface)  
  
- 추상메서드를 <u>딱 1개</u> 가지고 있는 인터페이스  
- SAM (Single Abstract Method) 인터페이스  
- `@FuncationInterface` 애노테이션을 가지고 있는 인터페이스  
  
```java
// Functional Interface example
@FunctionalInterface // 에노테이션을 갖고
public interface NewFunctionalInterface {

    void doIt(); // 추상메서드는 1개이다.  

    static void printName(String name) {
        System.out.println(name);
    }

    default void printAge(int age) {
        System.out.println(age);
    }
}
```  
  
> **추상메서드?**  
> 선언만 있고, 구현은 없는 메서드. `abstract` 키워드를 사용한다.  
> 인터페이스의 모든 메서드는 `public abstract`이며, 키워드는 **생략가능**하다.  
> 단, `default`, `staic` 메서드는 예외.
  
자바에서 함수형 프로그래밍은,  
- 함수(메서드)가 `First Class Object`여야 한다.  
- 또한, 해당 함수는 `Pure Fuction`이어야 한다.  
- 함수가 함수를 리턴하는 `High Order-Function`이 가능하다.  
- 함수가 참조하는 모든 객체는 `final`로 **불변성**을 갖는다.  
  
> **First Class Object?**  
> 함수의 매개변수 값, 반환 값으로 사용할 수 있다.  
> 변수나 자료구조에 저장할 수 있다.  
  
> **Pure Fuction?**  
> 함수 외부에서 정의된 값을 변경하지 않는다. 즉, 사이드 이펙트 ❌  
> 함수 외부에서 정의된 값을 참조하지 않는다. 즉, 상태가 ❌  
> <u>즉, 같은 입력값을 넣으면 같은 결과 값을 출력 하는 것.</u>  

### 람다 표현식(Lambda Expressions)  
  
- 함수형 인터페이스의 인스턴스를 만드는 방법으로 쓰일 수 있다.  
- 코드를 줄일 수 있다.  
- 메소드 매개변수, 리턴 타입, 변수로 만들어 사용할 수도 있다.  
  
```java  
public class RunMain {

	public static void main(String[] args) {
		int testNum = 20;

		// 기존에 사용하는 방법
		RunSomting runSomting = new RunSomting() {
			@Override
			public int doIt(int number) {
				//testNum++; 사이드 이펙트를 만들 수 없다.(함수밖에 있는 값을 변경하지 못한다.) 
				return number + 10;
			}
		};
		
		System.out.println(runSomting.doIt(10));

		// 람다 사용시
		RunSomting runSomtingLamda = (number) -> {
			//testNum++; 사이드 이펙트를 만들 수 없다.(함수 밖에 있는 값을 변경하지 못한다.) 
			return number + 20 + testNum;
		};
		System.out.println(runSomtingLamda.doIt(10));
	}
}
```
  
## 1.2. 자바에서 제공하는 함수형 인터페이스  
  
`java.util.fuction` 패키지에 정의되어 있으며,  
다음과 같이 자바에서 자주 사용할만한 함수 인터페이스를 미리 정의해두었다.  
  
**1. Function<T,R>**  
  
T 타입을 받아서 R 타입을 리턴하는 함수 인터페이스  
- R apply(T t)  
  
함수 조합용 메소드  
- andThen  
- compose  
  
```java
// examples Function<T,R>

import java.util.fuction.Function;

public class Plus10 implements Function<Integer, Integer>{
    @Override
    public Integer apply(Integer, Integer){
        return Integer + 10;
    }
}

public class RunSomethingImpl{
    public static void main(String[] args){
        Plus10 p10 = new Plus10();
        p10.apply(1);

        Function<Integer, Integer> p11 = (number) -> number + 11;
        p11.apply(1);
        Function<Integer, Integer> multiply2 = (number) -> number*2;
        
        // 1. andThen
        //먼저 p11을 진행하고, 그 다음에 multiply2 진행한다.  
        Function<Integer, Integer> Plus11Andmultiply2 = p11.andThen(multiply2); 
        Plus11Andmultiply2.apply(2);// (2+11)*2

        // 2. compose
        // multiply의 결과 값을 받아서 p11에 입력값으로 넘긴다.  
        Function<Integer, Integer> multiply2AndPlus11 = p11.compose(multiply2);
        multiply2AndPlus11.apply(2); // 2*2 + 11
    }
}

```
  
> **.apply( )**  
> Function 함수 인터페이스에서 갖고 있는 메서드  
> 매개 값과 리턴 값이 있는 경우에 사용하며,  
> <u>매개 값을 리턴값으로 매핑하는 역할</u>을 합니다. 
  
**2. BiFunction <T, U, R>**  
  
두 개의 값(T, U)를 받아서 R 타입을 리턴하는 함수 인터페이스  
- R apply(T t, U u)  
  
**3. Consumer <T\>**  
  
T 타입을 받아서 아무값도 리턴하지 않는 함수 인터페이스  
- void Accept(T t)  
  
```java
Consumer<Integer> printT = (i) -> System.out.println(i);
printT.accept(30);
```
  
> **.accept()**  
> Consumer 함수 인터페이스에서 갖고 있는 메서드  
> 리턴 값이 없는 경우에 사용하며,  
> 단지 매개 값을 소비하는 역할만 합니다.  
  
함수 조합용 메소드  
- andThen  

**4. Supplier <T\>**  
  
T 타입의 값을 제공하는 함수 인터페이스  
- T get()  
  
```java
Supplier<Integer> get10 = () -> 10;
System.out.println(get10.get());
```  
  
**5. Predicate <T\>**  
  
T 타입을 받아서 boolean을 리턴하는 함수 인터페이스  
- boolean test(T t)  
  
함수 조합용 메소드  
- And  
- Or  
- Negate : 결과값에 not을 붙이는 함수  
  
```java
Predicate<String> startWithJW = (s) -> s.startsWith("JW");
System.out.println(startsWithJW.test("JW"));
```  

**6. UnaryOperator <T\>**  
  
Function<T, R>의 특수한 형태로, 입력값 하나를 받아서 동일한 타입을 리턴하는 함수 인터페이스  
  
```java
UnaryOperator<Integer> plus10 = (number) -> number + 10;
```  
  
**7. BinaryOperator <T\>**  
  
BiFunction<T, U, R>의 특수한 형태로, 동일한 타입의 입렵값 두개를 받아 리턴하는 함수 인터페이스  
  
## 1.3. 람다 표현식  
<span style="color:grey">람다 표현식은 [이전 포스트](https://jaewanglee.github.io/java/ramda/)를 참고해도 좋습니다.</span>  
  
### 람다 기본 구성
  
다음과 같이 <u>인자와 바디</u>로 구성된다.  
  
```java
(인자) -> 바디 
```
  
**인자**  
  
- 인자가 없을 때: ( )  
- 인자가 한개일 때: (one) 또는 one  
- 인자가 여러개 일 때: (one, two)  
- 인자의 타입은 생략 가능, 컴파일러가 추론(infer)하지만 명시할 수도 있다. (Integer one, Integer two)  
  
**바디**  
  
- 화살표 오른쪽에 함수 본문을 정의한다.  
- 여러 줄인 경우에 `{ }`를 사용해서 묶는다.  
- 한 줄인 경우에 생략 가능, `return`도 생략 가능.  
  
### 변수 캡쳐(Variable Capture)와 Shadowing
  
**1. 로컬 변수 캡처**  
  
- `final`이거나 `effective final` 인 경우에만 참조할 수 있다.
- 그렇지 않을 경우 concurrency 문제가 생길 수 있어서 컴파일러가 방지한다.
  
```java
int baseNubmer = 10;
RunSomthing runSomething2 = (number) -> {
    return number + baseNumber;
};
baseNumber++; // 참조한 외부 변수 값을 변화시키는 코드 -> 에러 발생
runSomthing.doit(1);
```
  
**2. effective final**  
  
- 이것도 역시 자바 8부터 지원하는 기능으로 “사실상" final인 변수. 
- final 키워드 사용하지 않은 변수를 익명 클래스 구현체 또는 람다에서 참조할 수 있다.

**3. Shadowing**  
  
- 익명 클래스 구현체와 달리 **‘쉐도잉’**하지 않는다.  
- 익명 클래스는 새로 스콥을 만들지만, 람다는 람다를 감싸고 있는 스콥과 같다.
  
> **익명클래스(Anonymous Class)?**  
> 클래스의 선언과 객체의 생성을 동시에 하는 이름 없는 클래스 (일회용)  
> 이름이 없기 때문에 생성자를 가질 수 없다.  
> 단 하나의 클래스를 상속받거나, 단 하나의 인터페이스만을 구현할 수 있다.  
  
```java
public class testVariableCapture{
    public static void main(String[] args){
        testVariableCapture test = new testVariableCapture();
        test.run();
    }

    private void run(){
        // 공통점 : 여기서 정의한 baseNumber를 아래 세개의 클래스에서 전부 참조 가능하다.
        // = 변수 캡쳐(Variable Capture)
        int baseNumber = 10;

        // 차이점 : Shadowing 여부
        // 1. 로컬 클래스
        class LocalClass{
            void printBaseNumber(){
                int baseNumber = 11;
                System.out.println(baseNumber); // 11
            }
        }

        // 2. 익명 클래스
        Consumer<Integer> integerConsumer = new Consumer<Integer> (){
            @Override
            public void accept(Integer baseNumber){
                //메소드 내의 baseNumber는 더이상 run 메소드에 정의한 baseNumber가 아니다.  
                System.out.println(baseNumber);
                // 10이 아닌, accept 메소드가 입력받은 값을 리턴한다.  
            }
        }

        // 즉, 1과 2는 내부클래스에 별도의 scope이 생성되고,  
        // 생성된 scope에 정의된 변수가 상위 scope 변수를 가리는 Shadowing이 발생한다.  

        // 3. Lambda
        // IntConsumer = Consumer 인터페이스 중 정수형을 받는 인터페이스
        IntConsumer printInt = (numeber) -> {
            // int baseNumber = 11;
            // 컴파일 에러 발생. 동일한 scope에서 두 번 정의했기 때문.
            System.out.println(nubmer + baseNumber);
        };
        printInt.accept(10);
        // lambda를 사용할 경우, 별도의 scope이 생성되지 않는다.
        // 따라서 외부 변수를 재정의 할 수 없으며, Shadowing도 불가하다.  
    }
}
```
  
  
## 1.4. 메소드 레퍼런스
  
람다가 하는 일이 기존 메소드 또는 생성자를 호출하는 거라면,  
메소드 레퍼런스를 사용해서 매우 간결하게 표현할 수 있다.  
  
**메소드 참조하는 방법**  
  
|스태틱 메소드 참조|타입::스태틱 메소드|
|특정 객체의 인스턴스 메소드 참조|객체 레퍼런스::인스턴스 메소드|
|임의 객체의 인스턴스 메소드 참조|타입::인스턴스 메소드|
|생성자 참조|타입::new|
  
- 메소드 또는 생성자의 매개변수로 람다의 입력값을 받는다.  
- 리턴값 또는 생성한 객체는 람다의 리턴값이다.  
  
```java
public class testMethodReference {
    public static void main(String[] args){
        
        // 생성자 참조하기 - 1. 기본 생성자.
        // 입력 값은 없으나 리턴 값은 존재하는 인터페이스 : Supplier  
        Supplier<Greeting> newGreeting = Greeting::new;
        // Supplier만으로 객체를 생성하는 것은 아님. get 메소드 적용해야함.
        Greeting greeting0 = newGreeting.get();

        //생성자 참조하기 - 2. 인자 값을 받는 생성자.
        // 문자열을 받아서 객체를 리턴하므로 Function 사용
        Function<String, Greeting> newGreeting2 = Greeting::new;
        Greeting funcGreeting = newGreeting2.apply("createdByFunction");
        funcGreeting.getName(); // "createdByFunction"

        //인스턴스 메소드 참조하기
        Greeting greeting1 = new Greeting();
        UnaryOperator<String> hello = greeting1::hello;
        System.out.println(hello.apply("this is Name"));//"hello this is name"

        //static 메소드 참조하기 
        UnaryOperator<String> hi = Greeting::hi;
    }
}
```
  
```java
public class testInstanceMethodReference{
    public static void main(String[] args){
        String [] names = {"name1", "name2", "name3"};
        //정렬 기준을 지정하는 영역에 Comparator 인터페이스를 적용하는 기존 방식
        Arrays.sort(names, new Comparator<String>(){
            @Override
            public int compare(String o1, String o2){
                return 0;
            }
        });

        // lambda 형식
        Arrays.sort(names, (Comparator<String> (o1,o2)->0));
        // 다시 말해, 메소드 참조 방법이 사용 가능하다.  
        // name1, name2 비교 후 결과 리턴, name2, name3 비교 후 리턴...
        // 즉, 임의의 인스턴스에 해당 메소드를 참조하는 형태
        Arrays.sort(names, String::compareToIgnoreCase);
    }
}
```
  
끝-!