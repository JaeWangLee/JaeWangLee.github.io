---
title: "[더 자바8] 섹션4. Optional"
excerpt: "백기선의 더 자바, Java8 내용입니다."
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - 백기선
  - Java8
  - Optional
  - IntelliJ
last_modified_at: 2021-05-05 21:00:20
---

# 섹션4. Optional
<span style="color:grey">[백기선님의 더 자바, Java 8]강의 내용을 정리한 자료입니다.</span>
  
## 4.1. Optional 소개
  
자바 프로그래밍에서 NullPointerException을 종종 보게 되는 이유  
> null을 리턴하니까! && null 체크를 깜빡했으니까!  
  
Optional 이전 `NullPointerException`의 해결방식은 다음과 같았다.
- Null 체크를 한다. (에러 발생할 가능성이 ⬆️)  
  
```java
        // 해결방안 1. 옛날 방식
        OnlineClass spring_boot = new OnlineClass(1, "spring boot", true);
        Progress progress = spring_boot.getProgress();
        if(progress != null) {
            System.out.println(progress.getStudyDuration);
        }
        // 에러를 만들기 좋은 코드. 
        // 첫번째 이유. Null체크를 빼먹을 수 있어서.
        // 두번째 이유. Null을 리턴하는 것 자체가 문제
```  
  
- 리턴 값이 null일 경우 Exception을 주었다.  
> Exception은 해당 오류가 발생하기까지의 stackTrace 데이터를 리턴하는데, 이게 불필요한 때가 있다.  
  
- Null을 받은 클라이언트 코드 측에서 대응작업을 진행하였다.
  
### Optional
오직 값 한 개가 들어있을 수도 없을 수도 있는 컨네이너.
  
```java
public Optional<ReturnObject> getReturnObject(){
    // ofNullable : 인자값 (여기서는 returnObject)가 null 일 수도 있을 때 사용
    // null 일 경우 Optional 안에 빈 값을 넣은 것과 동일하다.
    return Optional.ofNullable(returnObject);
    
    // of:인자 값이 절대 null이 아닌 경우 사용. null을 넣으면 NullPointerException을 반환한다.
    // return Optional.of(returnObject);
}
```
  
**※ 주의할 것**  
- <u>리턴값으로만</u> 쓰기를 권장한다. (메소드 매개변수 타입, 맵의 키 타입, 인스턴스 필드 타입으로 쓰지 말자.)  
  
```java
public void OptionalParameter(Optional<Object> obj){
    // Optional 안에 실제 객체가 있는지 확인하는 코드가 필요
    obj.ifPresent(p -> System.out.print(p));
    // 하지만 위 코드도 인자인 obj를 Null로 주면 NullPointerException 발생한다.
    // Null에 ifPresent() 메소드를 호출하려 하기 때문이다. 
}
```
- Optional을 리턴하는 메소드에서 null을 리턴하지 말자. 필요시 `Optional.empty()`를 쓰자.  
- 프리미티브 타입용 Optional을 따로 있다. OptionalInt, OptionalLong,...  
  > 프리미티브 타입이 들어오면 안에서 boxing, unboxing이 벌어져 성능에 안좋은 영향을 준다.  
- Collection, Map, Stream, Array, Optional은 Optional로 감싸지 말 것.
  
## 4.2. Optional API
  
Optional 만들기
- Optional.of()
- Optional.ofNullable()
- Optional.empty()
  
Optional에 값이 있는지 없는지 확인하기
- isPresent()
- isEmpty() (Java 11부터 제공)
  
Optional에 있는 값 가져오기
- get()
- 만약에 비어있는 Optional에서 무언가를 꺼낸다면??
  
Optional에 값이 있는 경우에 그 값을 가지고 ~~를 하라.
- ifPresent(Consumer)
- 예) Spring으로 시작하는 수업이 있으면 id를 출력하라.
  
Optional에 값이 있으면 가져오고 없는 경우에 ~~를 리턴하라.
- orElse(T)
- 예) JPA로 시작하는 수업이 없다면 비어있는 수업을 리턴하라.
  
Optional에 값이 있으면 가져오고 없는 경우에 ~~를 하라.
- orElseGet(Supplier)
- 예) JPA로 시작하는 수업이 없다면 새로 만들어서 리턴하라.
  
Optional에 값이 있으면 가져오고 없는 경우 에러를 던져라.
- orElseThrow()
  
Optional에 들어있는 값 걸러내기
- Optional filter(Predicate)
  
Optional에 들어있는 값 변환하기
- Optional map(Function)
- Optional flatMap(Function): Optional 안에 들어있는 인스턴스가 Optional인 경우에 사용하면 편리하다.
  
```java
import java.util.List;

public class App{
    public static void main(String [] args){
        List<OnlineClass> springClasses = new ArrayList<>();
        springClasses.add(new OnlineClass(1, "spring boot", true));
		springClasses.add(new OnlineClass(2, "spring data jpa", true));
		springClasses.add(new OnlineClass(3, "spring mvc", false));
		springClasses.add(new OnlineClass(4, "spring core", false));
		springClasses.add(new OnlineClass(5, "rest api developement", false));

        // findFirst : "spring*"없을 수도 있으니 Optional<객체> 형태로 받는다.
        Optional<OnlineClass> optionalOnlineClass = springClasses.stream()
            .filter(oc -> oc.getTitle().startsWith("spring"))
            .findFirst();

        //get() : 값 꺼내기 없으면 NoSuchElementException - 런타임 에러
        Optional<OnlineClass> optionalOnlineClass = springClasses.stream()
            .filter(oc -> oc.getTitle().startsWith("jpa"))
            .findFirst();
        optionalOnlineClass.get( ); // 에러 발생
        // get 보다는 밑의 API를 활용하자.

        // isPresent() : null 여부 확인, JDK11부터는 isEmpty()도 지원
        // ifPresent(Consumer<Object>) : 값이 있으면 작업 수행
        optionalOnlineClass.ifPresent(oc -> System.out.println(oc.getTitle()));

        // orElse(인스턴스)
        // 값이 있으면 할당하고, 없으면 인스턴스를 수행한다. 여기서는 OnlineClass라는 인스턴스를 넣어야 함.
        // 단, 값이 있어서 orElse 부분이 반영되지 않는다 해도, createNewClass() 메소드는 무조건 실행됨
        // 즉, 이미 만들어져 있는 것(상수)을 가져올 경우는 orElse가 적합하다.  
        OnlineClass test = optionalOnlineClass.orElse(createNewClass());
        System.out.println(test.getTitle());

        // orElseGet(Supplier)
        // 무조건 메소드가 실행되는 상황을 막기 위한 코드.
        // else에 걸려서 무조건 실행이 필요한 경우에만 코드를 실행한다. 
        // 즉, 동적으로 새로운 객체를 생성해 실행해야 할 경우 orElseGet이 적합.
        optionalOnlineClass.orElseGet(()->createNewClass());
        
        // lambda를 사용할 경우
        optionalOnlineClass.orElseGet(testStreamExample::createNewClass);

        // orElseThrow(Supplier)
        // 없을 경우 원하는 형태의 Exception 던지는 코드
        optionalOnlineClass.orElseThrow(()->{return new IllegalStateException();});
        // Supplier는 생략 가능하다. 여기서는 IllegalStateException

        // filter()
        // 값이 있다는 전제하에 실행되는 메소드
        // return Optional<>. 없으면 빈 optional 객체 반환.
        // 즉, 없으면 아무일도 일어나지 않는다.
        optionalOnlineClass.filter(oc -> oc.getId() > 10);

        // map() : optional이 담고 있는 타입이 달라짐.
        // 만약 map으로 꺼낸 타입이 다시 Optional이면.. 몇 번을 꺼내야하는 번거로움.
        Optional<Optional<String>> s = optionalOnlineClass.map(OnlineClass::getOptionalString);
        Optional<String> optionalString = s.orElse(Optional.empty());
        
        // 이 경우 flatMap 메소드를 사용
        // c.f) stream의 flatMap은 input은 1개이지만, output은 여려 개일 경우 사용
        Optional<String> optionalStringByFlatMap = optionalOnlineClass.flatMap(OnlineClass::getOptionalString);
    }
}
```
  
끝-!