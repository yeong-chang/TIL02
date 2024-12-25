Generic
===

제네릭(Generic)은 자바에서 다양한 데이터 타입을 지원하면서도 코드의 중복을 줄이고, 컴파일 타임에 타입 안전성을 보장하기 위한 기능 입니다. 제네릭을 사용하면 클래스나 메서드에서 사용할 데이터 타입을 외부에서 지정할 수 있어, 코드의 재사용성과 안정성을 크게 향상시킬 수 있습니다.
- **타입 안전성(Type Safety)**: 제네릭을 사용하면 **컴파일 시점에 타입을 체크**할 수 있어, 잘못된 타입의 데이터가 사용될 경우 컴파일 에러가 발생합니다. 이를 통해 런타임 오류를 줄일 수 있습니다.

- **형변환 생략**: 제네릭을 사용하면 객체를 저장할 때 따로 형변환(casting)을 할 필요가 없습니다.

- **코드 재사용성**:  제네릭을 통해 다양한 타입을 지원하는 클래스나 메서드를 쉽게 정의할 수 있으며, 코드 중복을 줄이고 유영한 프로그래밍이 가능합니다.
  **제네릭 타입(Generic Type):**

**제네릭 타입(Generic Type)은 자바에서 클래스,인터페이스, 메서드를 작성할 때, 구체적인 타입을 명시하지 않고 나중에 필요할 때 타입을 지정할 수 있도록 하는 기능.**   
(T,E,K,V와 같은 대문자로 표현 합니다.)              
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




---
### 제네릭 타입 예제

|Car  

````java
package ch01;

public class Car {
    @Override
    public String toString() {
        return "Car []";
    }
}
````      

|Tv
````java

package ch01;

public class Tv {
    @Override
    public String toString() {
        return "Tv []";
    }
}

````

|Product      

````java

package ch01;

public class Product <T, M> {
    //T: type
    //M: model

    //필드
    private T type;

    private M model;


    //메서드
    public T getType() {
        return this.type;
    }

    public void setType(T type) {
        this.type = type;
    }

    public M getModel() {
        return model;
    }

    public void setModel(M model) {
        this.model = model;
    }

}

````

|Main

````java
package ch01;

public class Main {
    public static void main(String[] args) {

            Product<Tv, String> product01 = new Product<>();

            product01.setType(new Tv());
            product01.setModel("스마트TV");

            Tv tv = product01.getType();
            String tvModel = product01.getModel();
            System.out.println(tv+","+tvModel);

            System.out.println("===================================");

            Product<Car, String> product02 = new Product<>();
            product02.setType(new Car());
            product02.setModel("SUV");

            Car car = product02.getType();
            String carModel = product02.getModel();
            System.out.println(car+","+carModel);
    }
}
````  

|실행 결과
````java
Tv [],스마트TV
===================================
Car [],SUV
````


