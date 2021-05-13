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
![이미지](/assets/images/JAVA/Collections/col1.png)  
  
코드로 확인해보자.  
  
```java
package listPjt

import java.util.ArrayList;

public class MainClass{
    public static void main (String[] args){

        //ArrayList 객체 생성
        ArrayList<String> list = new ArrayList<String>();

        System.out.println("list.size : " + list.size());

        //데이터 추가
        list.add("Hello");
        list.add("Java");
        list.add("World");
        System.out.println("list.size : " + list.size());
        System.out.println("list : " + list);

        list.add(2, "Prgramming"); // 추가 2번째 idx에 추가됨. World는 3으로 밀림
        System.out.println("list : " + list);

        list.set(1,"C"); // 변경
        System.out.println("list : " + list);

        //데이터 추출
        String str = list.get(2);
        System.out.println("list.get(2) : " + str);
        System.out.println("list : " + list);

        //데이터 제거 
        str = list.remove(2);
        System.out.println("list.remove(2) : " + str);
        System.out.println("list : " + list);

        //데이터 전체 제거
        list.clear();
        System.out.println("list : " + list);

        //데이터 유무
        boolean b = list.isEmpty();
        System.out.println("list.isEmpty() :" + b);

        System.out.println("====================");

    }
}

```
## 25-2. Map
![이미지](/assets/images/JAVA/Collections/col2.png)  
  
key를 이용한다. 
key-value 구조이며, key는 유일한 값이다.  
코드로 확인해보자.  
  
```java
package mapPjt

import java.util.ArrayList;

public class MainClass{
    public static void main (String[] args){
        
        // HashMap 객체 생성
        HashMap<Integer, String> map = new HashMap<Integer, String>();
        System.out.println("map.size() : " + map.size());

        // 데이터 추가
        map.put(5, "Hello");
        map.put(6, "Java");
        map.put(7, "World");
        System.out.println("map : " + map);
        System.out.println("map.size() : " + map.size());

        map.put(8, "!!");
        System.out.println("map : " + map);

        //데이터 교체
        map.put(6, "C"); // 키값은 중복 될 수 없기에 Java 삭제됨
        System.out.println("map : " + map);

        //데이터 추출
        str = map.get(5);
        System.out.println("map.get(5) : " + map);
        
        //데이터 제거 
        map.remove(8);
        System.out.println("map.remove(8) : " + map);

        //특정 데이터 포함 유무
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