# studied during the project (BankSystem)
자바를 이용한 은행시스템 미니프로젝트중 생긴 문제 궁금증을 어떻게 해결했는지 정리한 글 이다.  
## 예외처리
회원 가입에 필요한 정보를 입력 받는데 입력받는 값은 아래의 코드와 같고
<pre>
public class MemberVO extends DTO {

    private String    accountNo   ;//계좌번호 <PK>
    private String    memberName  ;//이름
    private String    password    ;//비번
    private String    memberDob   ;//생년월일
    private double       balance     ;//잔액
}
</pre>

<pre>
public MemberVO createAccount() {
        MemberVO out = null;
        // 값 할당
        int no = 999991000 + ((int) (Math.random() * 100 + 1));
        String newAccountNo = Integer.toString(no);
        String accountNo = newAccountNo;// 계좌번호

        System.out.println("본인의 이름을 입력하세요:");
        String newMemberName=sc.nextLine();
        String memberName = newMemberName;// 이름

        System.out.println("설정하실 비밀번호를 입력하세요");
        String newPassword = sc.nextLine();
        String password = newPassword;// 비번

        System.out.println("본인의 생년월일을 입력하세요");
        String newMemberDob = sc.nextLine();
        String memberDob = newMemberDob;// 생년월일

        System.out.println("초기 입금 금액을 입력하세요");
        String newBalance = sc.nextLine();
        double balance = Double.parseDouble(newBalance);// 잔액

        out = new MemberVO(accountNo, memberName, password, memberDob, balance);

        return out;
    }
</pre>
입력값은 위처럼 스캐너 넥스트 라인으로 받아 주었다. try catch문으로 예외 처리를 하기전에 적절하지 않은값을
더 상세하게 설명해주기위해 정규식을 사용해 예외처리를 해주려했다. 간단하게 그림으로 보여주겠다.       
### 예외처리 방향성
![예외처리1.png](img%2F%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC1.png)               
처음에는 밑에 코드처럼 하나하나 하려니깐 코드가 너무 길고 지저분해서 예외처리를 담당하는 클래스를하나 파서!
메서드를 만들어보자고 생각했다.
<pre>
        System.out.println("본인의 이름을 입력하세요:");
        String newMemberName=sc.nextLine();// 이름
        try {
            if(!newMemberName.matches("^[가-힣a-zA-Z]+$")){
                System.out.println("한글 영어만 입력하세요");
            }

        }catch (Exception e) {
            System.out.println("오류가 발생했습니다.");
        }
</pre>
### 비밀번호 예외처리중 에러
순조롭게 만들어가고 잇었는데 에러가 발행했다 비밀번호에 숫자만 입력했는데 에러문자가 발생했다...       
![예외처리2.png](img%2F%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC2.png)

![예외처리3.png](img%2F%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC3.png)           
공부해보니 <mark>"^[0-9]{4}$"</mark> 는 0~9를 몇번반복할건인지를 {}안에 표시해야한다고 한다.
<pre>
if(!input.matches("^[0-9]{4}$")){
                     System.out.println(title+"는 숫자만 입력해주세요.");
                 }
                 <mark>else if (input.length()!=4){</mark>
                     System.out.println(title+"는 4자 입니다.");
                 }
             }
</pre>
하지만 나는 위 코드처럼 4글자가 아닐때는 다른 출력값을 주고싶었는데 위와같은 정규식을 사용하면
<mark>else if (input.length()!=4)</mark> 이 조건식까지 갈수가 없다..   
^[0-9]$ ,"^[0-9]{4}$", "^[0-9]+$" 총 3개의 정규식을 찾아냈다.      
#### ^[0-9]$     
이 표현식은 0부터9까지 딱 한자리수가 있는 문자열인지 판단하는 정규식이다. 딱 한자리만 판단하는건 내가유도하는
방식과 맞지 않다

#### "^[0-9]{4}$"
이 표현식은 0부터 9까지 4번 반복하는 문자열인지를 판단하는 정규식이다. 4글자를 입력하는것을 유도하는것은 맞지만
위에 말 한것처럼 나는 4글자가 아닐때는 다른 출력값을 주고싶다.

#### "^[0-9]+$"
이 표현식은 하나 이상의 숫자로 이루어진 문자열인지를 판단하는 정규식이다. 예를 들어, "1", "12", "3456" 등 여러 자리 숫자를 모두 포함합니다.
이러면 위에 코드에 정규식을 넣으면 길이가 4가 아니라면 다른 값을 출력하는것도 가능하다.
내가 유도하는 방식과 일치하다.

---
또 금액을 예외 처리하는중 첫글자에 0또는 다른문자가 들어가면 안되니 첫글자는 1~9까지만 입력 가능하도록 예외를 처리 하려했는데
밑에처럼 에러가 났다

![예외처리4.png](img%2F%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC4.png)           
### 금액 예외 처리중 에러 발생
비밀번호 에러난것과 비슷하게 한글자만 매칭해줘서 이러난 에러였다.                
<pre>
if(title == "금액"){
                if(!input.matches("^[1-9][0-9]{0,7}$")){
                    System.out.println(input+"원은 존재하지 않는 금액입니다. 금액은 0부터 시작할수 없습니다");
                }
                else if(input.length()>8){
                    System.out.println(input+"원은 큰금액이라 신분증을 제출해주세요");
                }
             }
</pre>
#### 금액 예외 처리 에러 해결 방법
이런식으로 1번째 글자는 1~9까지 올수있도록 그다음 7글자는 0~9까지 입력할수 있도록 정규식을 빠꿔주니 해결 되었다.         
하지만 8글자보다 많은 글자가 올때도 위에 if문만 타서 if문과 else if문의 순서를 바꿔주었다.               
<pre>
if(title == "금액"){
                 if(input.length()>8){
                     System.out.println(input+"원은 큰금액이라 신분증을 제출해주세요");
                 }
                 else if(!input.matches("^[1-9][0-9]{0,7}$")){
                    System.out.println(input+"원은 존재하지 않는 금액입니다. 금액은 0부터 시작할수 없습니다");
                }
             }
</pre>
이제 길이가 8글자보다 클때는 if문을 타고 첫글자에 1~9가 오지 않거나 뒤에값이 이상한 값일때는 else if 문을 타도록 만들었다.

## 첫번째 날 예외처리 코드 
<pre>
public class CatchMethod {

    public void check_Input(String input , String title){
        try{
             if(title == "이름"){
                 if (!input.matches("^[가-힣a-zA-Z]+$")){
                     System.out.println(title+"은 한글과 영어로 입력해주세요.");
                 }
                 else if (input.length()<2) {
                     System.out.println(title+"은 최소 2글자 이상을 입력해주세요.");
                 }
                 else if (input.length()>20){
                     System.out.println(title+"은 최대 20글자 까지 입력할 수 있습니다.");
                 }
             }
             if(title == "비밀번호")
                 if(!input.matches(  "^[0-9]+$")){
                     System.out.println(title+"는 숫자만 입력해주세요.");
                 }
                 else if (input.length()!=4){
                     System.out.println(title+"는 4자 입니다.");
                 }

             if(title == "생년월일"){
                 if (input.length() != 8) {
                     System.out.println("생년월일은 8자리 입니다.");
                 }
                 String year = input.substring(0, 4);
                 String month = input.substring(4, 6);
                 String day = input.substring(6, 8);
                 if(!input.matches("^[0-9]+$")){
                     System.out.println(title+"은 숫자만 입력해주세요.");
                 }  else if (!year.matches("^(19[0-9]{2}|200[0-9])$")){
                     System.out.println("혹시 나이가 몇살이신가요...");
                 }
                 else if (!month.matches("^(0[1-9]|1[0-2])$")){
                     System.out.println(month+"월은 존재하지 않습니다. 1월부터12월중 입력해주세요.");
                 }
                 else if (!day.matches("^(0[1-9]|[12][0-9]|3[01])$")){
                     System.out.println(day+"일은 존재하지 않습니다. 1일부터31일중 입력해주세요");
                 }
             }
             if(title == "금액"){
                 if(input.length()>8){
                     System.out.println(input+"원은 큰금액이라 신분증을 제출해주세요");
                 }
                 else if(!input.matches("^[1-9][0-9]{0,7}$")){
                    System.out.println(input+"원은 존재하지 않는 금액입니다. 금액은 0부터 시작할수 없습니다");
                }
             }
        }catch (Exception e) {
           System.out.println("오류가 발생했습니다.");
       }
    }
}
</pre>