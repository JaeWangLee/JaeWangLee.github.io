---
title: "[더 자바8] 섹션5. Date와 Time"
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
last_modified_at: 2021-05-09 22:15:20
---

# 섹션5. Date와 Time
<span style="color:grey">[백기선님의 더 자바, Java 8]강의 내용을 정리한 자료입니다.</span>
  
## 5.1. Date와 Time 소개
   
### 자바 8에 새로운 날짜와 시간 API가 생긴 이유
- 그전까지 사용하던 java.util.Date 클래스는 mutable 하기 때문에 thead safe하지 않다.
- 클래스 이름이 명확하지 않다. Date인데 시간까지 다룬다.
- 버그 발생할 여지가 많다. (타입 안정성이 없고, 월이 0부터 시작한다거나..)
- 날짜 시간 처리가 복잡한 애플리케이션에서는 보통 Joda Time을 쓰곤했다.

```java
public static void main(String[] args) throws InterruptionException{

    // 예전에 이들을 사용했고, 불편했었음..
    Date date = new Date();
    Calendar calendar = new GregorianCalendar();
    SimpleDateFormat dateFormat = new SimpleDateFormat();
    
    // 그 이유는 
    // 1. 이름이 잘 작명이 되어있지 않다. 
    // date이지만 time도 표현됨
    long time = date.getTime();
    System.out.println(date): // Sun May 9 22:31:37 PDT 2021 출력
    System.out.println(time); // 1592779548765 출력

    // 2. Mutable한 객체라 멀티 Thread에서 사용하기 어렵다.
    Date date = new Date();
    Thread.sleep(1000*3);
    Date after3Seconds = new Date();
    System.out.println(after3Seconds); // 3초 뒤 시간이 나옴
    after3Seconds.setTime(time);
    System.out.println(after3Seconds); // 3초 전 시간이 나옴

    // 3. 버그 발생할 여지가 많다.
    Calendar jaewangBirthday = new GregorianCalendar(1993,10,21);
    // month에다가 숫자를 쓰면 안됨. 왜냐하면 0이 1월로 되어있음..
    // 10 대신 OCTOBER로 해놔야함

}
```

### 자바 8에서 제공하는 Date-Time API
- JSR-310 스팩의 구현체를 제공한다.
- 디자인 철학
  - Clear
  - Fluent
  - Immutable
  - Extensible

### 주요 API
- 기계용 시간 (machine time)과 인류용 시간(human time)으로 나눌 수 있다.
- 기계용 시간은 EPOCK (1970년 1월 1일 0시 0분 0초)부터 현재까지의 타임스탬프를 표현한다.
- 인류용 시간은 우리가 흔히 사용하는 연,월,일,시,분,초 등을 표현한다.
- 타임스탬프는 Instant를 사용한다.
- 특정 날짜(LocalDate), 시간(LocalTime), 일시(LocalDateTime)를 사용할 수 있다.
- 기간을 표현할 때는 Duration (시간 기반)과 Period (날짜 기반)를 사용할 수 있다. 
- DateTimeFormatter를 사용해서 일시를 특정한 문자열로 포매팅할 수 있다.
  
## 5.2. Date와 Time API
  
### 지금 이 순간을 기계 시간으로 표현하는 방법
- `Instant.now()`: 현재 UTC (GMT)를 리턴한다.
- Universal Time Coordinated == Greenwich Mean Time
  
```java
    Instant now = Instant.now(); // 지금 시간 출력 
    System.out.println(now); //2021-05-09T23:53:33.110587Z. 기준시 UTC, GMT 
    System.out.println(now.atZone(ZoneId.of("UTC"))); // UTC기준으로 나옴

    ZonedDateTime zonedDateTime = now.atZone(ZoneId.systemDefault()); // Local time 출력
    System.out.println(zonedDateTime);
```
  
### 인류용 일시를 표현하는 방법
- `LocalDateTime.now()`: 현재 시스템 Zone에 해당하는(로컬) 일시를 리턴한다.
- `LocalDateTime.of(int, Month, int, int, int, int)`: 로컬의 특정 일시를 리턴한다. 
- `ZonedDateTime.of(int, Month, int, int, int, int, ZoneId)`: 특정 Zone의 특정 일시를 리턴한다.

```java
LocalDateTime now = LocalDateTime.now(); // Local 시간을 가져옴
LocalDateTime birthday = LocalDateTime.of(1993,OCTOBER, 21, 0, 0, 0);
ZonedDateTime nowInKorea = ZoneDateTime.now(ZoneId.of("Asia/Seoul"));
```
  
### 기간을 표현하는 방법
- `Period` / `Duration` . beteen()
  > `Period`는 Human용 시간을 비교할 때  
  > `Duration`은 Machine용 시간을 비교할 때  
  
```java
    // Human용 시간을 비교할 때
    LocalDate today = LocalDate.now();
    LocalDate thisYearBirthday = LocalDate.of(2021,OCTOBER,21);
    Period period = Period.between(today, birthDay);
    System.out.println(priod.get(ChronoUnit.DAYS));

    // Machine용 시간을 비교할 때
    Instant now = Instant.now();
    Instant plus = now.plus(10, ChronoUnit.SECONDS);
    Duration.between(now, plus); // 두 개의 차
    System.out.println(between.getSeconds);
```
  
### 파싱 또는 포매팅
- [미리 정의해둔 포맷 참고](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html#predefined)
- `LocalDateTime.parse(String, DateTimeFormatter);`
- `Dateteme`
  
```java
    DateTimeFormatter formatter = DateTimeFormatter.ofPattern("MM/dd/yyyy");
    LocalDate date = LocalDate.parse("07/15/1982", formatter);
    System.out.println(date);
    System.out.println(today.format(formatter));
```
### 레거시 API 지원
- `GregorianCalendar`와 `Date` 타입의 인스턴스를 `Instant`나 `ZonedDateTime`으로 변환 가능.
- java.util.TimeZone에서 java.time.ZoneId로 상호 변환 가능.

```java
    ZoneId newZoneAPI = TimeZone.getTimeZone("PST").toZoneId(); // 예전 API에서 최근 API로
    TimeZone legacyZoneAPI = TimeZone.getTimeZone(newZoneAPI); // 최근 API에서 예전 API로

    Instant newInstant = new Date().toInstant();
    Date legacyInstant = Date.from(newInstant);

    GregorianCalendar gregorianCalendar = new GregorianCalendar();
    LocalDateTime dateTime = gregorianCalendar.toInstant().atZone(ZoneId.systemDefault());
    GregorianCalendar.from() = GregorianCalendar.from(dateTime);
    
```
  
끝-!