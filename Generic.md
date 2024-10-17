Generic
===
제네릭이란 결정되지 않은 타입을  파라미터로 처리하고 실제 사용할 때  
파라미터를 구체적인 타입으로 대체시키는 기능이다
---
<pre>
public class Box{
    public ? content;
}
</pre>
클래스 Box에 다양한 타입의 내용물을 저장해야할떄 어떤 타입을 선언해야 할까?
<pre>
public class Box{
    public <mark>Object</mark> content;
}
</pre>
Object 타입은 모든 클래스의 최상위 부모 클래스이다.    
그렇기 때문에 content필드에는 어떤 객체든 대입이 가능하다.
```java
String content = (String) box.content;
```
Object 타입을 사용할시 (String)으로 매번 강제 형 변환 해줘야한다
````java
public class Box <T>{
    public T content;
}
````
T는 파라미터 타입을 의미하고 `<T>`에 생성할 클래스의 타입을 입력해준다
````java
Box<String> box = new Box<String>();
box.conten = "안녕하세요";
String content = box.content; //강제 타입 변환이 필요없다
````

````java
package ch13.sec01;

public class Box <T> {
    public T content;
}

````

````java
package ch13.sec01;

public class GenericExample {
    public static void main(String[] args) {
        //Box<String> box1 = new Box<String>();
        Box<String> box1 = new Box<>();
        box1.content ="안녕하세요";
        String str = box1.content; //box1.content를 강제 형변환 해주지 않아도 사용가능
        System.out.println(str);

        //Box<Integer> box2 =new Box<Integer>();
        Box<Integer> box2 = new Box<>();
        box2.content = 100;
        int value = box2.content; //box2.content를 강제 형변환 해주지 않아도 사용가능
        System.out.println(value);
    }
}
````
|실행 결과
<pre>
안녕하세요
100
</pre>

