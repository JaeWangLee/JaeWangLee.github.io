---
title: "[강의노트] 문자열 클래스"
excerpt: "Java 문자열 클래스에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - class
  - Ramda
  - 객체지향
  - 인프런
last_modified_at: 2021-04-18 22:42:20
---

# 24. 문자열 클래스
<span style="color:grey">[자바 프로그래밍 입문 강좌 (renew ver.) - 초보부터 개발자 취업까지!!]내용입니다.</span>

## 24-1. String 객체와 메모리?
![이미지](/assets/images/JAVA/Stringclass/sc1.png)
> 사실 메모리상에서는 클래스라기보다는 객체가 맞는 표현  
  
**새로운 메모리 주소로 이사를 한 것**  
즉, str의 주소는 다르다.  
그렇다면 첫번째 str은 GC에 의해 회수된다.  
하지만 회수되기 이전까지는 존재하므로 메모리 효율에 나쁜 영향을 준다.  
  
 ## 24-2. StringBuffer, StringBuilder
![이미지](/assets/images/JAVA/Stringclass/sc2.png)  
**기존의 메모리 공간에 추가를 한 것**  
StringBuffer는 Synchronize 기법을 사용하므로,
데이터가 순차적으로 하나씩 받게 된다.
그래서 속도가 좀 늦으나, 데이터가 안전하게, 유실되지 않는다.  
  
StringBuilder는 Synchronize 하지 않지만,
들어오는대로 받는다. 그래서 데이터 안전성이 떨어질 수 있다.  
  
### 실습  
  
```java
package testPjt;

public class MainClass{
    public static void main (String[] args){

        //String str = "JAVA";
        String str = new String("JAVA");
        System.out.println("str : " + str);
        
        str = str + "_8";
        System.out.println("str : " + str);

        //StringBuffer
        StringBuffer sf = new StringBuffer("JAVA");
        System.out.println("sf : " + sf);
        
        //맨 뒤에 추가하고 싶은 경우
        sf.append("world");  
        System.out.println("sf : " + sf);

        //길이를 알고 싶은 경우
        System.out.println("sf.length() : " + sf.length());

        //원하는 위치에 추가하고 싶은 경우
        sf.insert(sf.length(), "~~~"); // 문자열의 끝에 추가
        System.out.println("sf : " + sf);

        //원하는 위치 삭제하고 싶은 경우
        sf.delete(4,8); // [4]~[7]까지 삭제됨
        System.out.println("sf : " + sf);

        //StringBuilder
        StringBuilder sb = new StringBuilder("JAVA World!!");
        System.out.println("sb : " + sb);

    }
}
```
  
**결과**  
![이미지](/assets/images/JAVA/Stringclass/sc3.png)  
끝-!😋