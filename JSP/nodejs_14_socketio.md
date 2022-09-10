# Node.js 14: Socket io

> Wed Sep 7, 2022

---



Node.js 로 실시간 채팅을 구현해봅시다.

클라이언트는 서버와 채팅합니다. 서버에서 받고 보내는 작업을 해주는 것을 socket.io 라고 합니다. 

각각에게 보내거나 특정그룹에게만 보낼수도 있도록 구현이 되어있는 것이 socket.io 입니다. 

보낸다는 것은 이벤트 발생, 서버에서 받으려면 이벤트 대기 그리고 다시 보내기 위해 이벤트 발생이 필요합니다. 



VScode 를 열어서 socketio 라는 폴더를 생성하고 chat_form_1.html 뷰페이지를 만듭니다.

#### chat_form_1.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Echo Chatting...</h1>
    <div>
        message : <input type="text" id="msg"/>
                  <input type="button" id="send" value="Send"/>
    </div>
    <hr/>
    <!-- 서버에서 받은 메세지 표시 -->
    <div id="chatMessage" style="background-color: #ddd">111</div>
</body>
</html>
```

#### 

```sh
// socket.io@2 모듈 설치
cd socketio
npm install socket.io@2
```



#### node14_socketio.js

```js
// socket.io@2 모듈 설치
// >npm install socket.io@2

var http = require('http');
var fs = require('fs');
var socketio = require('socket.io');

var server = http.createServer((request, response)=>{
    // 만약 url 주소가 chatEcho 이면 fileRead 를 해서 보내
    if(request.url=='/chatEcho'){
        fs.readFile(__dirname+"/chat_form_1.html", "utf-8", function(error, chatFormData){
            response.writeHead(200, {'Content-Type': 'text/html; charset=utf-8'});
            response.end(chatFormData);
        })
    }
});

server.listen(10030, function(){
    console.log("start chat server... http://127.0.0.1:10030/chatEcho"); // 여기서 chatEcho 는 위에있는 chatEcho 를 실행시키는것
})

// ================= 통신프로그램 구현 ========================
// 1) 소켓 소버를 생성하고 실행한다. 
var io = socketio.listen(server); // 클라이언트와 연결되어있는 서버를 매개변수로 받는다.

// 메세지를 주고 받을 수 있기때문에 이벤트 생성과 대기 둘다 필요하다.
// 2) 접속을 대기하는 이벤트를 생성, 'connection 이벤트가 발생하면 실행될곳'
//           이벤트종류, 콜백함수
io.sockets.on('connection', function(socket){  // hello 이벤트로 보낸 문자를 받으려면 이 소켓 필요
    // 3) 클라이언트가 보낸 문자열을 보낸 이벤트
    //      이벤트종류, 이벤트를 받으면 실행할 콜백함수
    socket.on('hello', function(msg){
        console.log("서버가 받은 문자 ==>" + msg);

        // 4) 서버에서 클라이언트로 문자보내기
        // [1] 서버와 클라이언트가 1:1 통신
        socket.emit('echo', 'server message : '+ msg);
    });
});
```



#### chat_form_1.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
    <script src="/socket.io/socket.io.js"></script>
    <script>
        var socket; // 소켓변수가 필요하다.
        $(function(){  // 제이쿼리로 구현
            // 1. 서버에 접속한다.
            socket = io.connect(); // 서버연결시도 // 서버에 연결해서 소켓을 구한다.
            // 2. 클라이언트가 서버로 정보보내기 (이벤트 발생)
            //          이벤트종류, 서버로 보낼문자
            socket.emit('hello', '안녕! 나 클라이언트야');

            // 서버가 보낸 문자받기
            socket.on('echo', function(eventMsg){
                $("#chatMessage").append(eventMsg+"<br/>"); // 메세지 div 에 찍어주기
            });
            
            // 입력한 정보를 서버로 보내기
            $("#send").click(function(){
                messageSend();
            });
            $("#msg").keyup(function(){
                if(event.keyCode==13){ // ascii 코드 엔터키
                    messageSend();
                }
            });

            function messageSend(){
                socket.emit('hello', $("#msg").val());
                $("#msg").val('');
                $("#msg").focus();
            }
        })
    </script>
</head>
<body>
    <h1>Echo Chatting...</h1>
    <div>
        message : <input type="text" id="msg"/>
                  <input type="button" id="send" value="Send"/>
    </div>
    <hr/>
    <!-- 서버에서 받은 메세지 표시 -->
    <div id="chatMessage" style="background-color: #ddd"></div>
</body>
</html>
```

