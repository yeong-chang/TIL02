# 입출력 스트림(Input/Output Stream)    
-프로그램과 외부장치(파일, 네트워크. 콘솔 등)강의 데이터를
입력하거나 출력하는 과정에서 사용되는 일련의 데이터 흐름 입니다. 스트림은 데이터가 연속적으로 전달되는 통로를 의미 합니다.   

입출력 스트림(Input/Output Stream)은크게 두 가지 유형으로 나뉩니다.

- 입력스트림(Input Stream):외부로부터 프로그램 내부로 데이터를 읽어 들이는 역할을 합니다.
예를들어, 파일로부터 데이터를 읽거나, 네트워크 소켓으로 데이터를 받아올 때 사용됩니다.
- 출력스트림(Out Stream):프로그램의 내부 데이터를 외부로 보내는 역할을 합니다. 예를들어,
데이터를 파일 쓰거나, 네트워크 소켓을 통해 데이터를 전송할 때 사용됩니다.

## 자바의 입출력 스트림 분류:
1. 바이트 스트림(Byte Stream):1바이트 단위로 데이터를 입출력 합니다. 주로 이미지, 동영상, 바이너리 파일 같은 데이터를 처리할 때 사용됩니다.

---
2. 문자 스트림(Character Stream): 2바이트(유니코드)단위로 문자를 입출력 합니다. 주로 텍스트파일 같은 문자 데이터를 처리할 때 사용됩니다. 

![데이터입출력1.png](img%2F%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9E%85%EC%B6%9C%EB%A0%A51.png)   



---
###  바이트 스트림(Byte Stream)
바이트 스트림(Byte Stream)은 자바에서 1바이트 단위로 대이터를 입출력 하는 스트림 입니다. 주로 이미지, 오디오, 비디오 파일과 같은 바이너리
데이터를 처리하는 데 사용 됩니다. 문자 스트림과 달리, 바이트 스트림은 문자가 아닌 모든 유형의 데이터를 바이트 단위로 처리하기 떄문에 
비텍스트 데이터를 다루기에 적합합니다.

바이트 스트림의 주요 특징:         
- 데이터 단위: 데이터를 8비트(1바이트)단위로 처리 합니다.
- 바이너리 파일 처리: 이미지, 오디오, 비디오 파일과 같은 바이너리 데이터를 처리하는 데 사용 됩니다.
- 입출력 방식: 데이터 입출력은 바이트 단위로 이루어지며, 이는 모든 파일의 기본 단위인 바이트 단위로 데이터를 처리하기 때문에 텍스트와 비 텍스트 
데이터를 모두 처리할 수 있습니다

바이트 스트림의 기본 클래스:         
바이트 스트림은 Input/Output Stream 클래스를 상속받아 다양한 기능을 제공합니다. 
#### InputStream
InputStream의 주요 클래스:    
![데이터입출력2.png](img%2F%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9E%85%EC%B6%9C%EB%A0%A52.png)

InputStream의 주요 메서드:           
![데이터입출력3.png](img%2F%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9E%85%EC%B6%9C%EB%A0%A53.png)               
````java
package ch03;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;

public class Input {
    public static void main(String[] args) {
        try {
            InputStream is = new FileInputStream("C:/Users/acorn4/Desktop/Db.txt");

            while (true){
                int data = is.read();
                if(data == -1) break;
                System.out.println(data);
            }
            is.close();
        }catch (FileNotFoundException e) {
            e.printStackTrace();
        }catch (IOException e) {
            e.printStackTrace();
        }
    }
}

````

---
#### OutputStream
OutputStream의 주요클래스:         
![데이터입출력0.png](img%2F%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9E%85%EC%B6%9C%EB%A0%A50.png)       
OutputStream의 주요 메서드:       
![데이터입출력4.png](img%2F%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9E%85%EC%B6%9C%EB%A0%A54.png)

|OutputStream 
````java
package ch03;

import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;

public class Output {
    public static void main(String[] args) {
        try{
            OutputStream os = new FileOutputStream("C:/Users/acorn4/Desktop/Db.txt"); //바이트 출력스트림 생성

            byte[] array = {10 ,20 , 30, 40 , 50};

            os.write(array,1 , 3);  //1번인덱스부터, 3번 인덱스까지 출력, array만 주면 전체 출력
            
            os.flush(); //내부 버퍼에 잔류하는 바이트를 출력하고 버퍼를 비움
            os.close(); //출력 스트림을 닫아서 사용한 메모리를 해제

        }catch(IOException e){
            e.printStackTrace();
        }
    }
}

````
---
### 문자기반 스트림(Character Stream)             
Reader와Writer는 자바의 문자 스트림을 다루는 기본 클래스 입니다.Read는 문자를 읽는데 사용되며, Writer는 문자를 쓰는 데 사용 됩니다.
이 두 클래스는 문자기반 스트림의 입출력을 처리하는데 사용되며, 바이트 스트림과 다르게 문자열을 다루기에 더 효율적입니다.

#### Writer
문자 출력 스트림 주요 클래스:      
![데이터입출력6.png](img%2F%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9E%85%EC%B6%9C%EB%A0%A56.png)       
Writer 주요 메서드:      
![데이터입출력7.png](img%2F%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9E%85%EC%B6%9C%EB%A0%A57.png)           
![데이터입출력8.png](img%2F%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9E%85%EC%B6%9C%EB%A0%A58.png)       
|Write
````java
package ch03;

import java.io.FileWriter;
import java.io.IOException;
import java.io.Writer;

public class Write {
    public static void main(String[] args) {
        try {
            //문자 기반 출력 스트림 생성
            Writer writer = new FileWriter("C:/Users/acorn4/Desktop/Db.txt");

            //1 문자씩출력
            char a ='A';
            writer.write(a);
            char b ='B';
            writer.write(b);

            //char 배열 출력
            char[] arr = {'C','D','E'};
            writer.write(arr);

            //문자열 출력
            writer.write("FGH");

            //버퍼에 잔류하고 있는 문자들을 출력하고, 버퍼를 비움
            writer.flush();

            //출력 스트림을 닫고 메모리 해제
            writer.close();

        }catch (IOException e){
            e.printStackTrace();
        }
    }
}

````
#### Reader
문자 입력 스트림 주요 클래스:           
![데이터입출력9.png](img%2F%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9E%85%EC%B6%9C%EB%A0%A59.png)                  
Reader 주요 메서드:      
![데이터입출력10.png](img%2F%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9E%85%EC%B6%9C%EB%A0%A510.png)         
|Read           
````java
package ch03;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.io.Reader;

public class Read {
    public static void main(String[] args) {
        try {
            Reader reader = null;

            //1문자씩 읽기
            reader = new FileReader("C:/Users/acorn4/Desktop/Db.txt");
            while (true) {
                int data = reader.read();
                if (data == -1) break;
                System.out.print((char) data);
            }
            reader.close();
            System.out.println();

            //문자 배열로 읽기
            reader = new FileReader("C:/Users/acorn4/Desktop/Db.txt");
            char[] data = new char[100];
            while (true) {
                int num = reader.read(data);
                if (num == -1) break;
                for (int i = 0; i < num; i++) {
                    System.out.print(data[i]);
                }
            }
            reader.close();


        }catch (FileNotFoundException e) {
            e.printStackTrace();
        }catch (IOException e) {
            e.printStackTrace();
        }
    }
}

````
### 보조 스트림      
자주사용하는 보조스트림        
![데이터입출력11.png](img%2F%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%9E%85%EC%B6%9C%EB%A0%A511.png)         
#### BufferedReader
````java
package ch04;
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
public class CharacterConvertStream {
    public static void main(String[] args) {
        // 파일 경로 설정
        String filePath = "C:/Users/acorn4/Desktop/Db.txt";

        // 파일을 읽기 위한 BufferedReader
        try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
            String line;

            // 파일에서 한 줄씩 읽어오기
            while ((line = reader.readLine()) != null) {
                System.out.println(line); // 읽은 내용을 출력
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

````

