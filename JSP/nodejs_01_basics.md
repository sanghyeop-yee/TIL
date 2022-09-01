# Node.js 01 : Basic

> Mon Aug 29, 2022

---





서버생성

server.listen 은 접속대기로써 사용자를 기다리는곳입니다. 

클라이언트가 접속하면 var server 가 실행이 되겠죠. 그게 request, 서버에서 클라이언트 보내는게 response 입니다.



#### node01_start.js

```js
// 노드 js : 이벤트기반 서버 프레임워크이다.
// 프로젝트 : dos>npm init -y
// 실행 : 터미널 > node 파일명.js
////////////////////////////////////////////////
// 1. 웹서버를 생성하기 위해서는 http 모듈을 객체 생성하여야 한다.
var http = require("http");

// 2. http 를 이용하여 서버프로그램을 구현한다.
var server = http.createServer(function(request, response){
    // request : 클라이언트 측에 서버로 정보 보내기

    // response : 서버에서 클라이언트 측으로 정보 보내기
    // 1. response에 header 정보 설정
    response.writeHead(200, {"Content-Type":"text/html; charset=utf-8"});
    // 2. response 에 컨텐츠 보내기
    response.write("<html><head><title>처음으로 하는 노드</title></head>")
    response.write("<body>");
    response.write("<h1>노드 start</h1>");
    response.write("<p>이벤트 기반 서버 프레임워크</p>");
    // 3. response 에 응답의 마지막을 표기하여야 한다.
    response.end("<b>끝!!!</b>");

});

// 3. 클라이언트의 접속을 대기하는 함수
server.listen(3000, "localhost", function(){
    console.log("server is running... http://localhost:3000");
});
```



Terminal > New Terminal 을 실행하여 

`node node01_start.js` 를 입력하고 서버연결을 확인합니다.





이번에는 GET 방식으로 전송된 데이터를 서버에서 받아봅시다.



```js
// 1. 웹서비스 객체 생성 / 서버를 생성
var http = require('http');
const { URLSearchParams } = require('url'); 

// 2. 서버생성 및 구현
http.createServer(function(request, response){
    // GET 방식으로 전송된 데이터를 서버에서 받기
    let data = new URLSearchParams(request.url.substring(2));
    console.log(data);

    response.writeHead(200, {"Content-Type": "text/html; charset=UTF-8"});
    response.write("<h1>서버에서 받은 데이터</h1>");
    response.write("<ul><li>이름=" + data.get("username") + " </li>");
    response.write("<li>연락처=" + data.get("tel") + "</li>");
    response.write("<li>나이=" + (parseInt(data.get("age")) + 10) + "</li>");
    response.write("</ul>");


}).listen(10000, function(){ // 3. 접속대기
    console.log("server start... http://127.0.0.1:10000?username=홍길동&tel=010-7777-9999&age=25");
});


```

