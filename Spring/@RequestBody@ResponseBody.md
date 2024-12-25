# @RequestBody, @ResponseBody 

@RequestBody는 클라이언트가 보내는 HTTP 요청 본문을 자바 객체로 변환하는 데 사용됩니다.  
@ResponseBody는 서버에서 반환하는 자바 객체를 HTTP 응답 본문에 포함시켜, 주로 JSON 형태로 클라이언트에게 응답하는 데 사용됩니다.

## @RequestBody
HTTP 요청 본문(body)에서 데이터를 읽어서 자바 객체로 변환하는 데 사용됩니다.  
<mark>클라이언트 -> 서버 요청을한다 (요청)</mark>
<pre>
@RestController
public class UserController {

    // POST 요청에서 JSON 데이터를 받아서 처리
    @PostMapping("/createUser")
    public String createUser(@RequestBody User user) {
        // 클라이언트가 보낸 JSON 데이터를 자바 객체로 변환
        System.out.println("User name: " + user.getName());
        System.out.println("User age: " + user.getAge());

        return "User " + user.getName() + " has been created!";
    }
}
</pre>


## @ResponseBody
자바 객체를 HTTP 응답 본문에 직렬화하여 클라이언트에 반환하는 데 사용됩니다.  
보통 JSON이나 XML 형태로 응답이 반환됩니다.  
<mark>서버 -> 클라이언트 요청을한다 (응답)</mark>

<pre>
@RestController
public class UserController {

    // GET 요청에 대해 JSON 응답 반환
    @GetMapping("/getUser")
    @ResponseBody
    public User getUser() {
        // 서버에서 객체를 JSON 형식으로 응답
        return new User("Alice", 25);
    }
}</pre>


### @RequestBody,@ResponseBody모두 동기처리에 더 많이 사용하는 이유    
요청, 응답 하는 과정을 진행하는동안엔 아무런 작업을 하지 않고 요청, 응답이 완료하기 전까지는
아무런 동작도 하지않고 응답이 완료한후 다른 작업을 하는 과정이 동기 처리 과정이기 떄문에
자연스럽게 동기처리에 더 많이 사용한다.

### 정리 
@RequestBody는 클라이언트가 서버에 요청을 할떄 사용하고   
@ResponseBody는 서버가 클라이언트에게 응답할때 사용합니다.