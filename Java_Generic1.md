Collection Framework
===
---
List_Collection
---

<pre>
public class Board {
    private String subject;
    private String content;
    private String writer;
}
</pre>
<pre>
package J01;

import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
    //ArrayList 컬렉션 생성
        List < Board> list = new ArrayList<>();


        //객체추가
        list.add(new Board("과목1","내용1","글쓴이1"));
       

        //저장된 총 객체 수 얻기
        int size = list.size();
        System.out.println("총 객체 수:"+size);
        System.out.println();
    }
}
</pre>
리스트에 제너릭타입의 Board를 입력했고
메인메서드에서 제너릭타입의 어레이리스트에 타입에 맞게 객체를 추가했다 
<pre>
//리스트에 객체를 여러개 추가함
list.add(new Board("과목1","내용1","글쓴이1"));
list.add(new Board("과목2","내용2","글쓴이2"));
list.add(new Board("과목3","내용3","글쓴이3"));
list.add(new Board("과목4","내용4","글쓴이4"));
list.add(new Board("과목5","내용5","글쓴이5"));
 int size = list.size();
System.out.println("총 객체 수:"+size);
        System.out.println();

//특정 인덱스의 객체 가져오기
Board b = list.get(2);
System.out.println(b.getSubject()+"\t"+ b.getContent()+"\t"+b.getWriter());
    }
</pre>
실행결과
<pre>
총 객체 수:5

과목3	내용3	글쓴이3
</pre>
위와 같이 특정 인덱스의 객체를 불러와 객체를 출력할수있다.
<pre>
for (int i = 0; i < list.size() ; i++) {
            Board b =list.get(i);
            System.out.println(b.getSubject()+"\t"+ b.getContent()+"\t"+b.getWriter());
        }
//특정 객체 삭제
        list.remove(2);
        //첫번째 remove에서 index값이 2인값을 지우고난후 index3인값이 index2값을채운다
        list.remove(2);

        for (Board b : list) {
            System.out.println(b.getSubject()+"\t"+ b.getContent()+"\t"+b.getWriter());
        }
</pre>
모든 객체를 출력하고 특정 객체를 삭제한후 삭제후 남은값 출력

------------------
Vector
--
---
Vector는 ArrayList와 동일한 내부 구조를 가지고 있다. 하지만 Vector는 동기화된 메서드로 구성되어있어  
멀티스레드가 동시에 Vector() 메소드를 실행할 수 없다.
---

### Vector 생성방법

---
```java
List<E> list = new Vector<E>(); //E에 지정된 타입의 객체만 저장
List<E> list = new Vector<>();  //E에 저장된 타입의 객체만 저장
List list = new Vector<E>();    //모든 타입의 객체를 저장
```

```java
package J02;

import J01.Board;

import java.util.List;
import java.util.Vector;

public class VectorExample {
    public static void main(String[] args) {
        //Vector클래스 생성
        List<Board> list = new Vector<>();

        //작업 스레드 객체 생성
        Thread threadA = new Thread(){
            @Override
            public void run() {
                //객체 1000개추가
                for (int i = 1; i <= 1000; i++) {
                    list.add(new Board("제목"+i,"내용"+i,"글쓴이"+i));
                }
            }
        };
        //작업 스레드 객체 생성
        Thread threadB = new Thread(){
            @Override
            public void run() {
                //객체 1000개추가
                for (int i = 1001; i <= 2000; i++) {
                    list.add(new Board("제목"+i,"내용"+i,"글쓴이"+i));
                }
            }
        };

        //작업 스레드 실행
        threadA.start();
        threadB.start();

        //작업 스레드들이 모두 종료될 때까지 메인 스레드를 기다리게 함
        try {
            threadA.join();
            threadB.join();
        }catch (Exception e){
        }
        //저장된 총 객체 수 얻ㄱ;
        int size = list.size();
        System.out.println("총 객체 수:"+size);
        System.out.println();
    }
}

```

<pre>총 객체 수:2000</pre>
작업 스레드를 실행해 list에 
2000개의 객체가 생성되었음을 알수있다  
Vector를 사용하지않고 List<Board> list = new ArrayList<>();를 사용하면   
ArratList는 두 스레드가 동시에 add()메소드를 호출할 수 있기때문에 경합이 발생할수있어
실행결과가 2000개로 나오지 않는다

```java
  List<Board> list = new Vector<>(); -> List<Board> list = new ArrayList<>();
```
<pre>
실행결과: 총 객체 수:1844
</pre>

LinkedList
--
----------

### LinkedList 생성방법

---
```java
List<E> list = new LinkedList<E>(); //E에 저장된 탑입의 객체만 저장
List<E> list = new LinkedList<>();  //E에 저장된 탑입의 객체만 저장
List list = new LinkedList();       //모든 타입의 객체를 저장
```

```java
package J03;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class LinkedListExample {
    public static void main(String[] args) {
        //ArrayList 컬렉션 객체 생성
        List<String> list = new ArrayList<String>();

        //LinkedList 컬렉션 객체 생성
        List<String> list2 = new LinkedList<String>();

        //시작 시간과 끝 시간을 저장할 변수 선언
        long startTime;
        long endTime;

        //ArrayList 컬렉션에 저장하는 시간 측정
        startTime = System.nanoTime();
        for (int i = 0; i < 10000; i++) {
            list.add(String.valueOf(i));
        }
        endTime = System.nanoTime();
        System.out.printf("%-17s %8d ns \n","ArrayList 걸린시간: ",(endTime - startTime));

        //LinkedList 컬렉션에 저장하는 시간 측정
        startTime = System.nanoTime();
        for (int i = 0; i < 10000; i++) {
            list2.add(String.valueOf(i));
        }
        endTime = System.nanoTime();
        System.out.printf("%-17s %8d ns \n","LinkedKist 걸린시간: ",(endTime - startTime));
    }
}

```
<pre>
실행결과:
ArrayList 걸린시간:     983600 ns 
LinkedKist 걸린시간:    649300 ns 
</pre>
이처럼 LinkedList가 더 빠른 성능을 낸다 ArrayList가 느린 이유는
0번 인덱스에 개로운 객체가 추가되면서 기존 객체의 인덱스를 한칸씩 뒤로 미는 작업을 하기 때문이다
----
### Set 컬렉션

---
List 컬렉션은 저장 순서를 유지하지만, Set 컬렉션은 저장순서가 유지되지 않는다. 또한 객체를 중복해서 저장할 수 없고, 하나의 null만 저장할 수 있다.

![img.png](img%2Fimg.png)

---
### HashSet  

---
Set 컬렉션중 가장 많이 사용되는 것이 HashSet이다. 다음은 HashSet 컬렉션을 생성하는 방법이다.
<pre>
Set<E> set = new HashSet<E>();  //E에 지정된 타입의 객체만 저장
Set<E> set = new HashSet<>();   //E에 지정된 타입의 객체만 저장
Set set = new HashSet<>();      //모든 타입의 객체를 저장
</pre>
HasgSet은 동일한 객체는 중복 저장하지 않는다
```java
package J04;

import java.util.HashSet;
import java.util.Set;

public class HashSetExample {
    public static void main(String[] args) {
        //GashSet 컬렉션 생성
        Set<String> set = new HashSet<String>();

        //객체저장
        set.add("A");
        set.add("B");
        set.add("C");
        set.add("A");   //중복 객체이므로 저장하지 않음
        set.add("E");

        //저장된 객체 수 출력
        int size =set.size();
        System.out.println("총 객체수: "+size);
    }
}

```
<pre>
총 객체수: 4
</pre>

[Member.java](..%2FJava_practice%2Fsrc%2FJ05%2FMember.java)
```java
package J05;

public class Member {
    public String name;
    public int age;

    public Member(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public int hashCode() {
        return name.hashCode() + age;
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Member target) {
            return name.equals(target.name) && age == target.age;
        }else {
            return false;
        }
    }
}


```
hashCode()메서드를  name과 age값이 같으면 동일한 hashCode가 리턴하도록 오버라이드하고    
equls()메서르를 name과 age값이 같으면 true를 리턴하도록 오버라이드함

[HashSetExample.java](..%2FJava_practice%2Fsrc%2FJ05%2FHashSetExample.java)
```java
package J05;

import java.util.*;

public class HashSetExample {
    public static void main(String[] args) {
        //HashSet 컬렉션 생성
        Set<Member> set = new HashSet<Member>();

        //Member 갹채 저장
        set.add(new Member("홍길동",30));
        set.add(new Member("홍길동",30));

        //저장된 객체 수 출력
        System.out.println("총 객체 수 : "+set.size());
    }
}

```
 <pre>
실행결과: 총 객체 수 : 1
</pre>
인스턴스는 다르지만 동등 객체이므로 객체 1개만 저장

---
Set컬렉션은 인덱스로 객체를 검색해서 가져오는 메서드가 없다. 대신 객체를 한 개씩 반복해서 가져와야 하는데, 여기에는 두가지 방법이 있다.
하나는 다음과 같이 for문을 사용하는것이다.

```java
Set<E> set = new HashSet<E>();
for(E e : set){
        ...
        }
```
---
다음과 같이 Set 컬렉션의 iterator()메서드로 반복자를 얻어 객체를 하나씩 가져오는 방법도 있다.
```java
Set<E> set = new HashSet<>();
iterator<E> iterator = set.iterator();
```
| 리턴타입    | 메소드명      | 설명                                      |
|---------|-----------|-----------------------------------------|
| boolean | hasNext() | 가져올 객체가 있으면 true를 리턴하고 없으면 false를 리턴한다. |
| E       | next()    | 컬렉션에서 하나의 객체를 가져온다.                     |
| void    | remove()  | next()로 가져온 객체를 Set 컬렉션에서 제거한다.         |
사용방법
```java
while(iterator.hashNext()){
    E e = iterator.next();
        }
```
hasNExt() 메서드로 가져올 객체가 있는지 먼저 확인하고 있다면 next()로 객체를 가져오고 제거하고 싶다면
remove() 메서드를 사용한다.
```java
package J05;

import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class HashsSetExample {
    public static void main(String[] args) {
    //HashSet 컬렉션 생성
        Set<String> set = new HashSet<>();

        //객체 추가
        set.add("java");
        set.add("JDBC");
        set.add("JSP");
        set.add("Spring");

        //객체를 하나씩 가져와서 처리
        Iterator<String> iterator = set.iterator();
        while (iterator.hasNext()) {
            //객체를 하나 가져오기
            String element = iterator.next();
            System.out.println(element);
            if (element.equals("jSP")) {
                //가져온 객체를 컬렉션에서 제거
                iterator.remove();
            }
        }
        System.out.println();

        //객체 제거
        set.remove("JDBC");

        //객체를 하나씩 가져와서 처리
        for (String element : set) {
            System.out.println(element);
        }

    }
}
```
Iterator.next()를 반복문을 사용해 하나씩 반복해서 뽑아온다.    
Iterator.remove()를 사용해 값을 제거하고, Set.remove("객체") 를 사용해 제거하고     
반복문을 사용해서 출력해 보았다.
<pre>
실행결과:
java
JSP
JDBC
Spring

java
JSP
Spring        
</pre>
