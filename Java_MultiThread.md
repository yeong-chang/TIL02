# Multi Thread

### 멀티 쓰레드  
일반적으로 하나의 프로세서는 하나의 스레드를 가지고 작업을 수행한다.  
하지만 멀티 스레드는 하나의 프로세스 내에서 둘 이상의 스레드가 동시에 작업을 수행하는 것을 의미한다.

```java
import java.awt.*;

public class BeepPrintExample {
    public static void main(String[] args) {
        Toolkit toolkit = Toolkit.getDefaultToolkit();
        for (int i = 0; i < 5; i++) {
            toolkit.beep();  //비프음 발생
            try {
                Thread.sleep(500);} // 한번 실행후 쓰레드가 0.5초간 정지
            catch(Exception e) {

            }

        }
        for (int i = 0; i <5; i++) {
            System.out.println("띵");
            try{Thread.sleep(500); // 한번 실행후 쓰레드가 0.5초간 정지
            }catch(Exception e) {

            }
        }
    }
}
```
|실행결과
<pre>
띵
띵
띵
띵
띵
</pre>
메인 스레드가 두가지 작업을 처리 할 수 없음을 보여주는 예제이다    
두가지 작업을 처리한다면 0.5초 주기로 비프음을 발생시키며 띵을 프린팅 했겠지만   
실행결과는 비피음을 처리한 후 프린팅을 시작했다
---
다음은 메인 쓰레드와 작업 쓰레드를 이용해 
비프음 발생과 프린팅을 같이하도록 만들어 보겠다.

<pre>
package ch02.sec01.ex2;

import jdk.vm.ci.code.site.Mark;

import java.awt.*;

public class BeepPrintExample {
    public static void main(String[] args) {
      <mark>Thread thread = new Thread(new Runnable() {</mark>
            <mark>@Override</mark>
            <mark>public void run() {</mark>
                <mark>Toolkit toolkit = Toolkit.getDefaultToolkit();</mark>
               <mark> for (int i = 0; i < 5; i++) {</mark>
                    <mark>toolkit.beep();</mark>
                    <mark>try {</mark>
                       <mark> Thread.sleep(500);</mark>
                   <mark> } catch (Exception e) {</mark>

                    }
                }
            }
        });
        thread.start();
        for (int i = 0; i < 5; i++) {
            System.out.println("띵");
            try {
                Thread.sleep(500);
            } catch (Exception e) {

            }
        }
    }
}
</pre>
위 코드같이 작업 쓰레드에서 비프음을 발생 시키고 
메인쓰레드에서 프린팅을 하니 원래 목적같이 비프음과 프린팅을동시에 내는데 성공했다
---
### Thread 자식클래스 생성

생성방법:
<pre>
public class WorkerThread extends Thread{
    @Override
    public void rux() {
        //스레드기 실행할 코드
    }
}

//스레드 객체 생성
Thread thread = new WorkerThread();
</pre>
### Thread 이름
메인스레드는 main이라는 이름을 가지고  
작업 스레드는 자동적으로 Thread-n이라는 이름을 가진다.  
다른이름으로 설정하고싶다면
````java
thread.setName("쓰레드이름");
````
쓰레드 이름은 주로 디버깅할떄 어떤 쓰레드가 작업하는지 확인할 목적으로 주로 사용함  
확인방법은 정적메서드currentThread()로 스레드 객체를 얻고 get.Name메서드로 이름을 출력한다.
````java
Thread thread = Thread.currentThread();
System.out.println(thread.getName());
````
````java
package ch02sec04;

public class ThreadNameExamle {
    public static void main(String[] args) {
        Thread mainthread = Thread.currentThread();
        System.out.println(mainthread.getName()+"실행");

        for (int i = 0; i < 3; i++) {
          Thread threadA = new Thread(){
              public void run(){
                  System.out.println(getName()+"실행");
              }
          };
          threadA.start();
        }

        Thread chatThread = new Thread(){
          @Override
          public void run(){
              System.out.println(getName()+"실행");
          }
        };
        chatThread.setName("chat-thread");
        chatThread.start();
    }
}

````
|실행결과
<pre>
main실행
Thread-1실행
Thread-0실행
chat-thread실행
Thread-2실행
</pre>
### Thread 메서드 종류
![thread.png](img%2Fthread.png)         
### sleep
````java
package ch02sec05;

import java.awt.*;

public class SlppeExample {
    public static void main(String[] args) {
        Toolkit toolkit = Toolkit.getDefaultToolkit();
        for (int i = 0; i < 10; i++) {
            toolkit.beep();
            try {
                Thread.sleep(1000); //1초간 쓰레드 일시정지
            }catch (Exception e) {
                
            }
        }
    }
}

````
### join    

````java
package ch02sec05.ex02;

public class SumThread extends Thread {
    private long sum;

    public long getSum(){
        return sum;
    }

    public void setSum(long sum){
        this.sum = sum;
    }

    @Override
    public void run(){
        for (int i = 0; i <= 100; i++) {
            sum += i;
        }
    }
}

````

````java
package ch02sec05.ex02;

public class JoinExample {
    public static void main(String[] args) {
        SumThread sumThread = new SumThread();
        sumThread.start();
        try{
            sumThread.join(); //작업 스레드에서 작업이 끝날때까지 기다림
        }catch (InterruptedException e){

        }
        System.out.println("1~100 합: "+sumThread.getSum());
    }
}

````

### yield
````java
package ch02sec05.ex03;

public class WorkThread extends Thread {
    //필드
    public boolean work = true;

    public WorkThread(String name) {
        setName(name);
    }
    //메서드
    @Override
    public void run() {
        while (true) {
            if (work) {
                System.out.println(getName()+": 작업처리");
            }else {
                Thread.yield();
            }
        }
    }
}

````
````java
package ch02sec05.ex03;

public class YieldExample {
    public static void main(String[] args) {
        WorkThread workThreadA =new WorkThread("workThreadA");
        WorkThread workThreadB =new WorkThread("workThreadB");
        workThreadA.start();
        workThreadB.start();

        try {
            Thread.sleep(5000);
        }catch (InterruptedException e) {
        }
        workThreadA.work = false;

        try {
            Thread.sleep(10000);
        }catch (InterruptedException e) {
        }
        workThreadA.work = true;
    }
}

````
|실행결과
````java
workThreadA: 작업처리
workThreadB: 작업처리
workThreadA: 작업처리
        ...5초후
workThreadB: 작업처리
workThreadB: 작업처리
workThreadB: 작업처리
workThreadB: 작업처리
workThreadB: 작업처리
workThreadB: 작업처리
workThreadB: 작업처리
        ...10초후
workThreadA: 작업처리
workThreadB: 작업처리
workThreadA: 작업처리
````
실행결과와 같이 처음 5초동안은 Thread거 번갈아가며 실행하다가   
5초 뒤에 메인스레드가 ThreadA의 필드를 false로 변경하여 ThreadA가  
yield()매서드를 호출한다 따라서 ThreadB만 10초동안 실행하고    
다시 10초뒤엔 5초동안 두스레드가 같이 실행한다

### Thread동기화(Synchronized)       
동기화 메서드 선언하는 방법은 다음과 같이 synchronize 키워드를 붙이는것이다 
인스턴스와 정적 메서드 어디든 붙일수 있다.
<pre>
public synchronized void method(){
    //단 하나의 스레드만 실행하는 영역
}
</pre>
<pre>
public void method () {
//여러 스레드가 실행할 수 있는 영역
synchronized(공유객체){
//단 하나의 스레드만 실행하는 영역
}
//여러 스레드가 실행할 수 있는 영역
}
</pre>
![thread2.png](img%2Fthread2.png)   
User1Thread는 Calculator 객체의 memory 필드에 100을 먼저 저장하고 2초간 일시 정지 상태가 된다.
그동안 User2Thread가 memory 필드값을 50으로 변경한다. 2초가 지나 User1Thread가 다시 실행 상태가 되어 memory 필드의 값을 출력하면 User2Thread가 저장한 50이 나온다.

그런데 이렇게 하면 User1Thread에 저장된 데이터가 날아가버린다. 스레드가 사용 중인 객체를 다른 스레드가 변경할 수 없도록 하려면 스레드 작업이 끝날 때까지 객체에 잠금을 걸면 된다. 이를 위해 자바는 동기화(Synchronized) 메소드와 블록을 제공한다.  
![thread3.png](img%2Fthread3.png)   
객체 내부에 동기화 메소드와 동기화 블록이 여러 개가 있다면 스레드가 이들 중 하나를 실행할 때 다른 스레드는 해당 메소드는 물론이고 다른 동기화 메소드 및 블록도 실행할 수 없다. 하지만 일반 메소드는 실행이 가능하다.

|Calculator 
<pre>
package ch02sec06;

public class Calculator {
    private int memory;

    public int getMemory(){
        return memory;
    }

<mark>public synchronized void setMemory1(int memory){</mark>
    this.memory = memory;
    try {
    Thread.sleep(2000);
    }catch (InterruptedException e){
}
    System.out.println(Thread.currentThread().getName() + ": " + this.memory);
    }

    <mark>public void setMemory2(int memory){</mark>
        <mark>synchronized (this){</mark>
        this.memory = memory;
        try {
            Thread.sleep(2000);
        }catch (InterruptedException e){
        }
        System.out.println(Thread.currentThread().getName() + ": " + this.memory);
        }
    }
}
</pre>
강조된 부분은 각각 동기화 메서드, 동기화 블록 선언을한 부분이다

|User1Thred
````java
package ch02sec06;

public class User1Thread extends Thread {
    private Calculator calculator;

    public User1Thread() {
        setName("User1Thread");
    }

    public void setCalculator(Calculator calculator) {
        this.calculator = calculator;
    }

    @Override
    public void run() {
        calculator.setMemory1(100);
    }
}

````
|User2Thred 
````java
package ch02sec06;

public class User2Thread extends Thread {
    private Calculator calculator;

    public User2Thread(){
        setName("User2Thread");
    }

    public void setCalculator(Calculator calculator) {
        this.calculator = calculator;
    }

    @Override
    public void run() {
        calculator.setMemory2(50);
    }
}

````
|메인
````java
package ch02sec06;

public class SynchronizedExample {
    public static void main(String[] args) {
        Calculator calculator = new Calculator();

        User1Thread user1Thread = new User1Thread();
        user1Thread.setCalculator(calculator);
        user1Thread.start();

        User2Thread user2Thread = new User2Thread();
        user2Thread.setCalculator(calculator);
        user2Thread.start();
    }
}

````
|실행결과
````java
User1Thread: 100
User2Thread: 50
````    
위 코드처럼 동기화 했을떄 코드 실행의 흐름이다  
![thread4.png](img%2Fthread4.png)

### wait, notify
notify() 메소드를 호출해서 일시 정지 상태에 있는 다른 스레드를 실행 대기 상태로 만들고   
wait() 메소드를 호출하여 일시 정지 상태로 만든다.
주의할 점은 이 두 메소드는 동기화 메소드 또는 동기화 블록 내에서만 사용할 수 있다는 것이다.   
![thread5.png](img%2Fthread5.png)

|WorkObject
<pre>
package ch02sec07;

public class WorkObject {
    public synchronized  void methodA() {   //동기화 메서드
        Thread thread = Thread.currentThread();
        System.out.println(thread.getName()+": methodA 작업 실행");
      <mark>  notify(); //다른 스레드를 실행 대기 상태로 만듦 </mark>
        try {
          <mark>  wait();//자신의 스레드는 일시 정지 상태로 만듧</mark>
        } catch (InterruptedException e) {
        }
    }
    public synchronized void methodB() {
        Thread thread = Thread.currentThread();
        System.out.println(thread.getName()+": methodB 작업 실행");
      <mark> notify();</mark>
        try {
         <mark> wait();</mark>
        }catch (InterruptedException e) {
        }
    }
}

</pre>
|ThreadA
<pre>
package ch02sec07;

public class ThreadA extends Thread {
    private WorkObject workObject;

    <mark>public ThreadA(WorkObject workObject) { //공유작업 객체를 받음</mark>
       <mark> setName("ThreadA");// 스레드 이름 변경 </mark>
        this.workObject = workObject;
    }
    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            <mark>workObject.methodA(); //동기화 메서드 호출</mark>
        }
    }
}

</pre>
|ThreadB
<pre>
package ch02sec07;

public class ThreadB extends Thread {
    private WorkObject workObject;

  <mark>  public ThreadB(WorkObject workObject) { //공유 작업 객체를 받음</mark>
       <mark> setName("ThreadB"); //스레드 이름 변경</mark>
        this.workObject = workObject;
    }
    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
          <mark>  workObject.methodB(); //동기화 메서드 호출</mark>
        }
    }
}

</pre>
|WaitNotifyExample
<pre>
package ch02sec07;

public class WaitNotifyExample {
    public static void main(String[] args) {
      <mark>  WorkObject workObject = new WorkObject();  //공유 작업 객체 생성</mark>
        <mark>//작업 스레드 생성 및 실행</mark>
        ThreadA threadA = new ThreadA(workObject);
        ThreadB threadB = new ThreadB(workObject);
        
        threadA.start();
        threadB.start();
    }
}
</pre>
|실행결과
<pre>
ThreadA: methodA 작업 실행
ThreadB: methodB 작업 실행
ThreadA: methodA 작업 실행
ThreadB: methodB 작업 실행
ThreadA: methodA 작업 실행
ThreadB: methodB 작업 실행
...
</pre>
### 스레드 안전 종료
1. while
2. interrupt
### while

|조건이용
스레드가 while 문으로 반복 실행할 경우, 조건을 이용해서 run() 메소드의 종료를 유도할 수 있다. 
<pre>
public class XXXThread extends Thread {
    private boolean stop;
    
    public void run() {
       <mark> while (!stop) {
// 스레드가 반복 실행하는 코드;</mark>
            
        }
      <mark>  // 스레드가 사용한 리소스 정리</mark>
    }
}
</pre>
|PrintThread
<pre>
package ch02sec08;

public class PrintThread extends Thread {
    private  boolean stop;

  <mark>  public void setStop(boolean stop) { // 외부에서 값을 변경할수 있도록 setter선언</mark>
        this.stop = stop;
    }

    @Override
    public void run() {
        while(!stop){
            System.out.println("실행 중");
        }
        System.out.println("리소스 정리");
        System.out.println("실행 종료");
    }
}

</pre>
|SafeStopExample
<pre>
package ch02sec08;

public class SafeStopExample {
    public static void main(String[] args) {
        PrintThread printThread = new PrintThread();
        printThread.start();

        try {
            Thread.sleep(3000);
        }catch (InterruptedException e) {
        }
      <mark>  printThread.setStop(true); //PrintThread종료를 위해 stop의 필드값 변경</mark>
    }
}

</pre>
| 실행결과
<pre>
실행 중
실행 중
실행 중
실행 중
실행 중
...
리소스 정리
실행 종료
</pre>
실행결과와 같이 메인스레드 에서 3초후에 stop의 필드값을  true로 설정해 printThread를 종료한다

### interrupt
interrupt() 메소드는 스레드가 일시 정지 상태에 있을 때 InterruptedException 예외를 발생시키는 역할을 한다.

이것을 이용하면 예외 처리를 통해 run() 메소드를 정상 종료시킬 수 있다.  
![thread6.png](img%2Fthread6.png)       
Thread를 생성해서 start() 메소드를 실행한 후에 XThread의 interrupt() 메소드를 실행하면 XThread가 일시 정지 상태가 될 때 InterruptedException이 발생하여 예외 처리 블록으로 이동한다.

이것은 결국 while 문을 빠져나와 자원을 정리하고 스레드가 종료되는 효과를 가져온다.   

|PrintThread
<pre>
package ch02sec08.ex02;

public class PrintThread extends Thread {
    public void run() {
        try {
            while (true) {
                System.out.println("실행 중");
               <mark> Thread.sleep(500); //interruptedException이 발생할수 있도록 일시정지를 만듬</mark>
            }
        }catch (InterruptedException e) {
        }
        System.out.println("리소스 정리");
        System.out.println("실행 종료");
    }
}

</pre>
|InterruptExample
<pre>
package ch02sec08.ex02;

public class InterruptExample {
    public static void main(String[] args) {
        Thread thread = new PrintThread();
        thread.start();

        try{
            Thread.sleep(1000);
        }catch(InterruptedException e){
        }
        <mark>thread.interrupt();//interrupt() 메서드 호출</mark>
    }
}

</pre>
|실행결과
<pre>
실행 중
실행 중
리소스 정리
실행 종료
</pre>
PrintThread가 0.5초동안 멈춰 InterruptedException을 발생시키고       
메인메서드에서 1초후 서브thread를 interrupt해 PrintThread를 멈춘것이다

### DemonThread  
데몬(Daemon) 스레드는 주 스레드의 작업을 돕는 보조적인 역할을 수행하는 스레드이다. 주 스레드가 종료되면 데몬 스레드도 따라서 자동으로 종료된다.       

|AutoSavaThread
<pre>
package ch02sec09;

public class AutoSavaThread extends Thread {
    public void save(){
        System.out.println("작업 내용을 저장함.");
    }
    @Override
    public void run(){
        while (true){
            try {
                Thread.sleep(1000);
            }catch (InterruptedException e){
                break;
            }
            save();
        }
    }
}
</pre>      

| DemonExample
<pre>
package ch02sec09;

public class DemonExample {
    public static void main(String[] args) {
        AutoSavaThread autoSavaThread = new AutoSavaThread();
        <mark>autoSavaThread.setDaemon(true); //AutoSaveThread를 데몬스레드로 만들어줌</mark>
        autoSavaThread.start();

        try {
            Thread.sleep(3000);
        }catch (InterruptedException e) {
        }

        System.out.println("메인 스레드 종료");
    }
}
</pre>      
|실행 결과
<pre>
작업 내용을 저장함.
작업 내용을 저장함.
메인 스레드 종료
</pre>

###  ThreadPool     
병렬 작업 처리가 많아지면 스레드의 개수가 폭증하여 CPU 및 메모리 사용량이 늘어난다.
이렇게 병렬 작업 증가로 인한 스레드의 폭증을 막으려면 스레드풀(ThreadPool)을 사용하는 것이 좋다.        
![Thread07.png](img%2FThread07.png)

#### 스레드풀 생성
   자바는 스레드풀을 생성하고 사용할 수 있도록 java.util.concurrent 패키지에서 ExecutorService 인터페이스와 Executors 클래스를 제공하고 있다.

Executors의 다음 두 정적 메소드를 이용하면 간단하게 스레드풀인 ExecutorService 구현 객체를 만들 수 있다.             

![Thread08.png](img%2FThread08.png)     
  
<pre>
ExecutorService executorService =Executors.newCachedThreadPool();
</pre>      
다음 메서드는 작업 개수가 많아지면 새 스레드를 생성시켜 작업을 처리한다  
60초동안 스레드가 작업이 없으면 삭제된다
<pre>
ExecutorService executorService = Executors.newFixedThreadPool(5);
</pre>
다음 메서드는 작업 개수가 많아지면 최대 5개까지 스레드를 생성시켜 작업을 처리한다  
이 스레드풀의 특징은 생성된 스레드를 제거하지 않는다는 것이다 
   
![Thread10.png](img%2FThread10.png)       



---
#### 스레드풀 종료
   스레드풀의 스레드는 기본적으로 데몬 스레드가 아니기 때문에 main 스레드가 종료되더라도 작업을 처리하기 위해 계속 실행 상태로 남아 있다.

스레드풀의 모든 스레드를 종료하려면 ExecutorService의 다음 두 메소드 중 하나를 실행해야 한다.            

![Thread09.png](img%2FThread09.png)     

---
#### 스레드풀 생성과 종료
<pre>
package ch02sec10;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ExeutorServiceExample {
    public static void main(String[] args) {

       <mark> ExecutorService executorService = Executors.newFixedThreadPool(5);//스레드풀 생성</mark>

        
        //작업 생성과 처리 요청

      <mark>  executorService.shutdown();//스레드 풀 종료</mark>
    }
}
</pre>
위 코드처럼 스레드를 생성 및 종료함

---

#### 작업 생성과 처리 요청
하나의 작업은 Runnable 또는 Callable 구현 클래스로 표현한다. Runnable과 Callable의 차이점은  
작업 처리 완료 후 리턴값이 있느냐 없느냐이다.
다음은 Runnable과 Callable 구현 클래스를 작성하는 방법을 보여준다.         

![Thread10.png](img%2FThread10.png)    
작업 처리 요청이란 ExecutorService의 작업 큐에 Runnable 또는 Callable 객체를 넣는 행위를 말한다.
작업 처리 요청을 위해 ExecutorService는 다음 두 가지 메소드를 제공한다.

![Thread11.png](img%2FThread11.png)       
Runnable 또는 Callable 객체가 ExecutorService의 작업 큐에 들어가면 ExecutorService는 처리할 스레드가 있는지 보고, 없다면 스레드를 새로 생성시킨다.
스레드는 작업 큐에서 Runnable 또는 Callable 객체를 꺼내와 run() 또는 call() 메소드를 실행하면서 작업을 처리한다.    
<pre>
package ch02sec10.ex02;

import java.util.concurrent.Executor;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class RunnableExecuteExample {
    public static void main(String[] args) {
        //1000개의 메일 생성
        String[][] mails = new String[1000][3]; // 하나의 배열에 3가지 값이 들어가는 2차원 배열
        for (int i = 0; i < mails.length; i++) {
            mails[i][0] = "admin@gmail.com";
            mails[i][1] = "member"+i+"@gmail.com";
            mails[i][2] = "신상품 입고";
        }
        //ExecutorService 생성
        //최대 5개의 스레드가 동시에 작업을 수행할 수 있는 스레드 풀을 생성합니다.
       <mark> ExecutorService executorService = Executors.newFixedThreadPool(5);</mark>

        //이메일을 보내는 작업 생성및 처리 요청
        for (int i = 0; i < mails.length; i++) {
            final int index = i;
                  //객체를 인자로 받아 스레드 풀에서 실행합니다.
            <mark> executorService.execute(new Runnable() { </mark>
                @Override
                public void run() {
                    Thread thread = Thread.currentThread();
                    String from = mails[index][0];
                    String to = mails[index][1];
                    String content = mails[index][2];
                    System.out.println("["+thread.getName()+"}"+from+"==>"+to+": "+content);
                }
            });
        }
        //ExecutorService 종료 (작업이 끝나면 종료)
        executorService.shutdown();
    }
}

</pre>
---
<pre>
package ch02sec10.ex03;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class CallabeSubmitExample {
    public static void main(String[] args) {
        //ExecutorService 생성, 스레드가 5개인 풀 생성
        <mark>ExecutorService executorService = Executors.newFixedThreadPool(5);</mark>

        //계산 작업 생성 및 처리 요청
        for (int i = 1; i <= 100; i++) {
            final int index = i;
            Future<Integer> future = executorService.submit(new Callable<Integer>() {
               @Override
               public Integer call() throws Exception {
                   int sum = 0;
                   for(int i=1  ; i<=index ; i++){
                       sum += i;
                   }
                   Thread thread = Thread.currentThread();
                   System.out.println("["+thread.getName()+"] 1~"+ index +"합 계산");
                   return sum;
               }
            });
            try {
                int result = future.get();
                System.out.println("\t리턴값: "+result);
            }catch (Exception e) {
                e.printStackTrace();
            }
        }
        //ExecutorService 종료
        executorService.shutdown();
    }
}

</pre>
|실행 결과
<pre>
[pool-1-thread-1] 1~1합 계산
	리턴값: 1
[pool-1-thread-2] 1~2합 계산
	리턴값: 3
[pool-1-thread-3] 1~3합 계산
	리턴값: 6
...

[pool-1-thread-3] 1~98합 계산
	리턴값: 4851
[pool-1-thread-4] 1~99합 계산
	리턴값: 4950
[pool-1-thread-5] 1~100합 계산
	리턴값: 5050
</pre>