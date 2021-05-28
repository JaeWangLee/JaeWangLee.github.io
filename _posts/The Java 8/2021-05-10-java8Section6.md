---
title: "[더 자바8] 섹션6. CompletableFuture"
excerpt: "백기선의 더 자바, Java8 내용입니다."
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - 백기선
  - Java8
  - CompletableFuture
  - IntelliJ
last_modified_at: 2021-05-10 21:15:20
---

# 섹션6. CompletableFuture
<span style="color:grey">[백기선님의 더 자바, Java 8]강의 내용을 정리한 자료입니다.</span>
  
## 6.1. 자바 Concurrent 프로그래밍 소개
  
**Concurrent 소프트웨어**  
동시에 여러 작업을 할 수 있는 소프트웨어  
- 웹 브라우저로 유튜브를 보면서 키보드로 문서에 타이핑을 할 수 있다.  
- 녹화를 하면서 인텔리J로 코딩을 하고 워드에 적어둔 문서를 보거나 수정할 수 있다.  
  
**자바에서 지원하는 컨커런트 프로그래밍**  
멀티프로세싱 (ProcessBuilder)  
멀티쓰레드  
  
### 자바 멀티쓰레드 프로그래밍
메인쓰레드에서 다른 쓰레드를 만들 수 있는 방법은 크게 2가지가 있음.  
  
1. Thread 상속  
  
```java
    public static void main(String[] args) {
        HelloThread helloThread = new HelloThread();
        helloThread.start();
        System.out.println("hello : " + Thread.currentThread().getName());
    }

    static class HelloThread extends Thread {
        @Override
        public void run() {
            System.out.println("world : " + Thread.currentThread().getName());
        }
    }
```
  
2. Runnable 구현 또는 람다  
  
```java
Thread thread = new Thread(() -> {
        while(true){
            System.out.println("world : " + Thread.currentThread().getName());
            try {
            Thread.sleep(1000L); // 다른 쓰레드가 일을 먼저 할 수 있도록 한다. 
            } catch (InterruptedException e){ // 자는 동안 누군가 깨우면 여기로 들어옴.
                System.out.println("Exit!");
                return;
            }   
        }
    });
thread.start();

System.out.println("hello : " + Thread.currentThread().getName());
Thread.sleep(3000L);
thread.interrupt();
```
> [출력]
> hello : main  
> Thread: Thread - 0  
> Thread: Thread - 0  
> Thread: Thread - 0  
> Exit!  

### 쓰레드 주요 기능
1. **현재 쓰레드 멈춰두기 (sleep)**  
   - 다른 쓰레드가 처리할 수 있도록 기회를 주지만 그렇다고 락을 놔주진 않는다. (잘못하면 데드락 걸릴 수 있겠죠.)
2. **다른 쓰레드 깨우기 (interupt)**
   - 다른 쓰레드를 깨워서 interruptedExeption을 발생 시킨다. 
   - 그 에러가 발생했을 때 할 일은 코딩하기 나름. 종료 시킬 수도 있고 계속 하던 일 할 수도 있고.
3. **다른 쓰레드 기다리기 (join)**
   - 다른 쓰레드가 끝날 때까지 기다린다.
  
## 6.2. Executors
  
**고수준 (High-Level) Concurrency 프로그래밍**
- 쓰레드를 만들고 관리하는 작업을 애플리케이션에서 분리.
- 그런 기능을 Executors에게 위임.
  
**Executors가 하는 일**
- 쓰레드 만들기: 애플리케이션이 사용할 쓰레드 풀을 만들어 관리한다.
- 쓰레드 관리: 쓰레드 생명 주기를 관리한다.
- 작업 처리 및 실행: 쓰레드로 실행할 작업을 제공할 수 있는 API를 제공한다.
  
**주요 인터페이스**
- Executor: execute(Runnable)
- ExecutorService: Executor 상속 받은 인터페이스로, Callable도 실행할 수 있으며, Executor를 종료 시키거나, 여러 Callable을 동시에 실행하는 등의 기능을 제공한다.
- ScheduledExecutorService: ExecutorService를 상속 받은 인터페이스로 특정 시간 이후에 또는 주기적으로 작업을 실행할 수 있다.

**ExecutorService로 작업 실행하기**
```java
ExecutorService executorService = Executors.newSingleThreadExecutor();
executorService.submit(() -> {
    System.out.println("Hello :" + Thread.currentThread().getName());
});// 실행 후 다음 작업이 오기 전까지 대기. 즉, 프로세스가 죽지 않음.
```
  
**ExecutorService로 멈추기**
```java
executorService.shutdown(); // 처리중인 작업 기다렸다가 종료 graceful shutdown
executorService.shutdownNow(); // 당장 종료
```

**ScheduledExecuteService 활용하기**
```java
ScheduledExecutorService executorService = Executors.newSingleThreadScheduledExecutor();
executorService.schedule(getRunnable("Hello"),3,SECONDS);
// 3초 뒤에 실행하기

executorService.scheduleAtFixedRate(getRunnable("Hello"),1,2,SECONDS);
// 1초 기다렸다가 2초에 한번씩 출력하도록
```

**Thread 보다 더 많은 작업을 실행할 때**
```java
public static void main(String[] args){
    ExecutorService excutorService = Executors.newFixedThreadPool(2);
    excutorService.submit(getRunnable("Hello"));
    excutorService.submit(getRunnable("Keesun"));
    excutorService.submit(getRunnable("The"));
    excutorService.submit(getRunnable("Java"));
    excutorService.submit(getRunnable("Thread"));

    excutorService.shutdown();
}

private static Runnable getRunnable(String message){
    return () -> System.out.println(message + Thread.currentThread().getName());
}
```
>[출력]  
>Hellopool-1-thread-1  
>Keesunpool-1-thread-2  
>Thepool-1-thread-1  
>Javapool-1-thread-1  
>Threadpool-1-thread-1  
  
![이미지](/assets/images/TheJava8/Thread_1.png)
  
작업들을 `ExecutorService` 로 보낸다.  
ES 안에는 `BlockingQueue`가 존재한다.  
쓰레드가 너무 바빠서 처리하지 못한 작업들을 쌓아둔다.  
따라서 두 개가 돌더라도 대기 후에 처리된다.  
쓰레드를 생성하는 비용이 덜 드는 장점이 있다.  
  
**Fork/Join 프레임워크**
- ExecutorService의 구현체로 손쉽게 멀티 프로세서를 활용할 수 있게끔 도와준다.
  
## 6.3. Callable과 Future
  
**Callable**
- Runnable과 유사하지만 <u>작업의 결과를 받을 수 있다.</u>
  
```java
Callable<String> hello = () -> {
    Thread.sleep(2000L);
    return "Hello";
};

Future<String> helloFuture = executorService.submit(hello);
System.out.println(helloFuture.isDone()); // 완료시 true, 아니면 false
System.out.println("Started");
  
helloFuture.get(); // 만난 순간 결과값을 나올때까지 기다린다.(블록킹 콜) 
//helloFuture.cancel(true); 현재 진행중인 작업을 인터럽트 하면서 종료하는 것.
// cancel을 하면, get을 사용할 수 없으며, isDone은 무조건 true이다.  
  
System.out.println(helloFuture.isDone()); // 완료시 true, 아니면 false
System.out.println("End!!");
executorService.shutdown();
  
```  

**Future**
- 비동기적인 작업의 현재 상태를 조회하거나 결과를 가져올 수 있다.
- https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Future.html
  
**결과를 가져오기 get()**
  
```java
ExecutorService executorService = Executors.newSingleThreadExecutor();
Future<String> helloFuture = executorService.submit(() -> {
    Thread.sleep(2000L); 
    return "Callable";
});

System.out.println("Hello");
String result = helloFuture.get();
System.out.println(result);
executorService.shutdown();
```
  
- 블록킹 콜이다.
- 타임아웃(최대한으로 기다릴 시간)을 설정할 수 있다.
  
**작업 상태 확인하기 isDone()**
- 완료 했으면 true 아니면 false를 리턴한다.
  
**작업 취소하기 cancel()**
- 취소 했으면 true 못했으면 false를 리턴한다.
- parameter로 true를 전달하면 현재 진행중인 쓰레드를 interrupt하고 그러지 않으면 현재 진행중인 작업이 끝날때까지 기다린다.
  
**여러 작업 동시에 실행하기 invokeAll()**
- 동시에 실행한 작업 중에 제일 오래 걸리는 작업 만큼 시간이 걸린다.
  
**여러 작업 중에 하나라도 먼저 응답이 오면 끝내기 invokeAny()**
- 동시에 실행한 작업 중에 제일 짧게 걸리는 작업 만큼 시간이 걸린다.
- 블록킹 콜이다.
  
```java
Callable<String> hello = () -> {
    Thread.sleep(2000L);
    return "Hello";
};

Callable<String> hello = () -> {
    Thread.sleep(3000L);
    return "Java";
};

Callable<String> hello = () -> {
    Thread.sleep(1000L);
    return "Keesun";
};

List<Future<String>> futures = executorService.invokeAll(Arrays.asList(hello,java,keesun));
for(Future<String> f : futures){
    System.out.println(f.get());
}
  
executorService.shutdown();
  
```  
![이미지](/assets/images/TheJava8/Thread_2.png)
Hello가 2초, Java가 3초, Keesun이 1초  
`invokeAll`은 이 세개가 모두 끝날때 까지 기다린다.  
여기서는 Java가 끝날때까지 기다림.  
  
![이미지](/assets/images/TheJava8/Thread_3.png)
그렇다면 서버 1,2,3에 똑같은 파일을 다 복사해 놓았을 때 가져오라고 한다면?  
다 기다릴 필요가 없기 때문에. 이 경우에는 `invokeAny` 사용  
하나만 가져오면 끝내버림. Keesun이 출력  
  
## 6.4. CompletableFuture
  
자바에서 <u>비동기(Asynchronous) 프로그래밍</u>을 가능케하는 인터페이스.
- Future를 사용해서도 어느정도 가능했지만 하기 힘들 일들이 많았다.
  
**Future로는 하기 어렵던 작업들**
- Future를 외부에서 완료 시킬 수 없다. 취소하거나, get()에 타임아웃을 설정할 수는 있다.
- 블로킹 코드(get())를 사용하지 않고서는 작업이 끝났을 때 콜백을 실행할 수 없다.
- 여러 Future를 조합할 수 없다, 예) Event 정보 가져온 다음 Event에 참석하는 회원 목록 가져오기
- 예외 처리용 API를 제공하지 않는다.
  
**CompletableFuture**
- Implements Future
- Implements CompletionStage
  
**비동기로 작업 실행하기**
- 리턴값이 없는 경우: runAsync()
- 리턴값이 있는 경우: supplyAsync()
- 원하는 Executor(쓰레드풀)를 사용해서 실행할 수도 있다. (기본은 ForkJoinPool.commonPool())
  
**콜백 제공하기**
- thenApply(Function): 리턴값을 받아서 다른 값으로 바꾸는 콜백
- thenAccept(Consumer): 리턴값을 또 다른 작업을 처리하는 콜백 (리턴없이)
- thenRun(Runnable): 리턴값 받지 다른 작업을 처리하는 콜백
- 콜백 자체를 또 다른 쓰레드에서 실행할 수 있다.
  
**조합하기**
- thenCompose(): 두 작업이 서로 이어서 실행하도록 조합
- thenCombine(): 두 작업을 독립적으로 실행하고 둘 다 종료 했을 때 콜백 실행
- allOf(): 여러 작업을 모두 실행하고 모든 작업 결과에 콜백 실행
- anyOf(): 여러 작업 중에 가장 빨리 끝난 하나의 결과에 콜백 실행
  
**예외처리**
- exeptionally(Function)
- handle(BiFunction): 
  
```java
CompletableFuture<String> future = new CompletableFuture <> ();
future.complete("keesun"); // future의 기본값을 기선으로 정해주면서, 작업을 끝냄
System.out.println(future.get()); // get을 완전히 없앨 수 없다. 

CompletableFuture<String> future = CompletableFuture.completedFuture("keesun");
System.out.println(future.get());

//리턴이 없는 작업
CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
    System.out.println("Hello " + Thread.currentThread().getName());
});
future.get(); // 무슨일이 일어나려면 get이나 join을 해야하지~

//리턴이 있는 작업
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    System.out.println("Hello " + Thread.currentThread().getName());
    return "Hello";
});
System.out.println(future.get()); 
  
// 콜백을 주는 방법
// 1. thenApply
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    System.out.println("Hello " + Thread.currentThread().getName());
    return "Hello";
}).thenApply((s) -> { // 우리가 받은 결과 값을 다른 타입으로 변경 하는 것
    System.out.println(Thread.currentThread().getName());
    return s.toUpperCase();
});
System.out.println(future.get()); 

// 2. thenAccept
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    System.out.println("Hello " + Thread.currentThread().getName());
    return "Hello";
}).thenAccept((s) -> { // 우리가 받은 결과 값을 다른 타입으로 변경 하는 것
    System.out.println(Thread.currentThread().getName());
    System.out.println(s.toUpperCase());
});
future.get();

// 3. thenRun
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    System.out.println("Hello " + Thread.currentThread().getName());
    return "Hello";
}).thenRun(() -> { // 우리가 받은 결과 값을 다른 타입으로 변경 하는 것
    System.out.println(Thread.currentThread().getName());
});
future.get();

// 조합하기
// 1. thenCompose : 두 작업이 서로 이어서 실행하도록 조합
public static void main(String[] args) throws ExecutionException, InterruptedException{
    CompletableFuture<String> hello = CompletableFuture.supplyAsync(() -> {
        System.out.println("Hello " + Thread.currentThread().getName());
        return "Hello";
    });

    CompletableFuture<String> future = hello.thenCompose(App::getWorld);
    System.out.println(future.get());
}

private static CompletableFuture<String> getWorld(String message){
    return CompletableFuture.supplyAsync(() -> {
        System.out.println("World " + Thread.currentThread().getName());
        return "World";
    });
}
// 출력 
// Hello ForkJoinPool.commonPool-worker-3
// World ForkJoinPool.commonPool-worker-5
// Hello World


public static void main(String[] args) throws ExecutionException, InterruptedException{
    CompletableFuture<String> hello = CompletableFuture.supplyAsync(() -> {
        System.out.println("Hello " + Thread.currentThread().getName());
        return "Hello";
    });

    CompletableFuture<String> world = CompletableFuture.supplyAsync(() -> {
        System.out.println("World " + Thread.currentThread().getName());
        return "World";
    });

// 2. thenCombine() : 두 작업을 독립적으로 실행하고 둘 다 종료 했을 때 콜백 실행
    CompletableFuture<String> = hello.thenCombine(world, (h, w) -> h + " " + w);
    System.out.println(future.get());

// 3. allOf(): 여러 작업을 모두 실행하고 모든 작업 결과에 콜백 실행
// 모든 결과의 타입이 동일하지 않을 수도 있고, 
// 어떤 결과는 에러가 낫을 수도 있다. 
// 따라서 결과가 무의미 하고, 리턴이 중요함 

    List<CompletableFuture<String>> futures = Arrays.asList(hello, world);
    CompletableFuture[] futuresArray = futures.toArray(new CompletableFuture[futures.size()]);

    CompletableFuture<List<String>> results = CompletableFuture.allOf(futuresArray)
        .thenApply( v -> futures.stream()
                .map(CompletableFuture::join)
                .collect(Collectors.toList()));
    
    results.get().forEach(System.out::println);
    
// 4. anyOf(): 여러 작업 중에 가장 빨리 끝난 하나의 결과에 콜백 실행
    CompletableFuture<Void> future = CompletableFuture.anyOf(hello, world).thenAccept(System.out::println);
    future.get();

// 예외처리
// 1. exceptionally  
    boolean throwError = false;
    
    CompletableFuture<String> hello = CompletableFuture.supplyAsync(() -> {
        if(throwError){
            throw new IllegalArgumentException();
        }
        System.out.println("Hello " + Thread.currentThread().getName());
        return "Hello";
    }).exceptionally(ex -> {
        return "Error!";
    });

    System.out.println(hello.get());

// 2. handle
    CompletableFuture<String> hello = CompletableFuture.supplyAsync(() -> {
        if(throwError){
            throw new IllegalArgumentException();
        }
        System.out.println("Hello " + Thread.currentThread().getName());
        return "Hello";
    }).handle((resutl, ex) -> {
        if(ex != null){
            System.out.println(ex);
            return "Error!";
        }
        return result;
    });
}

```
  

끝-!