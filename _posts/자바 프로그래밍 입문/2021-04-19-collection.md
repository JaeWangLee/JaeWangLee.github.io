---
title: "[자바 입문] 25. Collections"
excerpt: "Java Collections에 대해 알아보자"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - java
  - class
  - Collections
  - 객체지향
  - 인프런
last_modified_at: 2021-04-19 21:18:20
---

# 25. Collections
<span style="color:grey">[자바 프로그래밍 입문 강좌 (renew ver.) - 초보부터 개발자 취업까지!!]내용입니다.</span>

## 25-1. List
![이미지](/assets/images/Java_프로그래밍_입문/25강/col1.png)  
  
코드로 확인해보자.  
  
```java
package listPjt

import java.util.ArrayList;

public class MainClass{
    public static void main (String[] args){

        //ArrayList 객체 생성
        ArrayList<String> list = new ArrayList<String>();

        System.out.println("list.size : " + list.size());

        //데이터 추가__add()
        list.add("Hello");
        list.add("Java");
        list.add("World");
        System.out.println("list.size : " + list.size());
        System.out.println("list : " + list);

        list.add(2, "Prgramming"); // 추가 2번째 idx에 추가됨. World는 3으로 밀림
        System.out.println("list : " + list);

        list.set(1,"C"); // 변경
        System.out.println("list : " + list);

        //데이터 추출__get()
        String str = list.get(2);
        System.out.println("list.get(2) : " + str);
        System.out.println("list : " + list);

        //데이터 제거__remove()
        str = list.remove(2); // 제거와 동시에 반환한다.
        System.out.println("list.remove(2) : " + str);
        System.out.println("list : " + list);

        //데이터 전체 제거__clear()
        list.clear();
        System.out.println("list : " + list);

        //데이터 유무__isEmpty()
        boolean b = list.isEmpty();
        System.out.println("list.isEmpty() :" + b);

        System.out.println("====================");

    }
}

```
## 25-2. Map
![이미지](/assets/images/Java_프로그래밍_입문/25강/col2.png)  
  
key를 이용한다. 
key-value 구조이며, key는 유일한 값이다.  
코드로 확인해보자.  
  
```java
package mapPjt

import java.util.ArrayList;

public class MainClass{
    public static void main (String[] args){
        
        String str = "";
        boolean b = false;
        // HashMap 객체 생성
        HashMap<Integer, String> map = new HashMap<Integer, String>();
        System.out.println("map.size() : " + map.size());

        // 데이터 추가__put()
        map.put(5, "Hello");
        map.put(6, "Java");
        map.put(7, "World");
        System.out.println("map : " + map);//5=Hello 의 형식으로 출력됨
        System.out.println("map.size() : " + map.size()); // 사이즈느 3임

        map.put(8, "!!");
        System.out.println("map : " + map);

        //데이터 교체__이미 있는 key값에 덮어쓰기
        map.put(6, "C"); // 키값은 중복 될 수 없기에 Java 삭제됨
        System.out.println("map : " + map);

        //데이터 추출__get(key)를 통해 추출 가능함. key는 배열의 index의 역할과 동일
        str = map.get(5);
        System.out.println("map.get(5) : " + map);
        
        //데이터 제거__remove()는 값을 반환하지 않는다. 
        map.remove(8);
        System.out.println("map.remove(8) : " + map);

        //특정 key의 데이터 포함 유무
        b = map.containsKey(7);
        System.out.println("map.containsKey(7) : " + b);

        b = map.containsValue("World");
        System.out.println("map.containsValue(\"World\") : " + b);

        // 데이터 전체 제거
        map.clear();
        System.out.println("map : " + map);

        // 데이터 유무
        b = map.isEmpty();
        System.out.println("map.isEmpty() : " + b);

    }
}

```

끝-!😋