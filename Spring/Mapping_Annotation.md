## @RequestMapping 
가장 일반적인 매핑 어노테이션으로, 특정 HTTP 요청을 처리하는 메서드에 매핑됩니다.
HTTP 메서드(GET, POST, PUT, DELETE 등)와 URI를 모두 지정할 수 있습니다.  

### @RequestMapping 의 주요 속성
method: HTTP 메서드(GET, POST)  
value 또는 path: 요청 URL 패턴  
params: 요청 파라미터 조건  
headers: 요청 헤더 조건  
consumes: 요청 본문의 미디어 타입  
produces: 응답 본문의 미디어 타입

<pre>
@RequestMapping(value="hello.do"
    ,method = RequestMethod.POST
    ,produces = "text/plain;charset=UTF-8"
			)
public String hello() {
    return "hello!";
}
</pre>

## @GetMapping 
@RequestMapping의 HTTP GET 요청 전용 버전입니다.  
GET 요청을 처리하는 메서드를 지정할 때 사용됩니다.
<pre>@GetMapping("/hello")
public String getHello() {
    return "Hello, GET!";
}
</pre>

## @PostMapping
@RequestMapping의 HTTP POST 요청 전용 버전입니다.  
클라이언트가 데이터를 서버에 제출할 때 사용됩니다.
<pre>
@PostMapping("/submit")
public String submitData(@RequestBody Data data) {
    return "Data submitted!";
}
</pre>

### @RequestMapping 사용이유  
여러 HTTP 메서드를 동시에 처리할 때 사용합니다
<pre>
@RequestMapping(value = "/common", method = {RequestMethod.GET, RequestMethod.POST})
public String common() {
    return "Handling both GET and POST requests!";
}
</pre>
Get과 Post 도 동일하게 파라미터 처리를 해줄수 있지만  
더 유연하게 파라미나 해더 처리를 하기위해 사용합니다
