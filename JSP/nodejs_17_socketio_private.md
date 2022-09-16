# Node.js 17: Socket IO Private

> Tue Sep 13, 2022

---



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
var id;
io.sockets.on('connection', function(socket){  // hello 이벤트로 보낸 문자를 받으려면 이 소켓 필요
    id = socket.id;
    console.log(id); // 아이디 생성
    // 3) 클라이언트가 보낸 문자열을 보낸 이벤트
    //      이벤트종류, 이벤트를 받으면 실행할 콜백함수
    socket.on('hello', function(msg){
        console.log("서버가 받은 문자 ==>" + msg);

        // 4) 서버에서 클라이언트로 문자보내기
        // [1] 서버와 클라이언트가 1:1 통신
        //socket.emit('echo', 'server message : '+ msg);
        // [2] 서버와 접속한 모든 클라이언트와 통신하기
        //io.sockets.emit('echo', 'public --> ' + msg);
        // [3] 브로드캐스트 통신방법 : 나를 제외한 모든 접속자에게 데이터 보내기
        //socket.broadcast.emit("echo", "broadcast --> "+ msg);
        // [4] private 통신방법 : 특정 클라이언트에게만 데이터를 보낸다. 
        //     id 가 전역변수이면 마지막접속자에게 데이터를 보낸다.
        io.sockets.in(id).emit('echo', 'private --> '+ msg); // 이 id 가지고 있는 접속자
    });
});
```

