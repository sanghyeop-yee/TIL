# Node.js 18: 채팅룸 만들기

> Tue Sep 13, 2022

---



#### chat_form_room.html

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>채팅방만들기</title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
    <script src="/socket.io/socket.io.js"></script>
    <style>
        #showMessage{display:none;}
    </style>
    <script>
        $(function(){
            // 방만들기
            $("#createRoom").click(function(){
                createRoom();
            });
            $("#roomName").keyup(function(){
                if(event.keyCode==13){
                    createRoom();
                }
            });
            function createRoom(){
                if($("#roomName").val()!=''){ // 방이름이 있을때
                    socket = io.connect(); // 서버와 연결

                    // 서버에 방 생성 : 방을 생성하기 위한 이벤트 발생
                    socket.emit('join', $("roomName").val());

                    // 방만들기 숨기고
                    //$("#roomDiv").css("display", "none");
                    $("#roomDiv").html("<h1>방이름 : " + $("#roomName").val()+"</h1>");
                    // 채팅하기 보여주기
                    $("#showMessage").css("display", "block")

                    // 서버에 보낸문자 받기
                    socket.on('response', function(msg){
                        $("#msgView").append("<li>"+msg+"</li>");
                    });
                }else{ // 방이름이 없을떄
                    alert("방이름을 입력후 방만들기를 하세요.");
                }
            }
            // 서버로 메세지 보내기
            $('#sendMsg').click(function(){
                sendMessage();
            });
            $("#msg").keyup(function(){
                if(event.keyCode==13){
                    sendMessage();
                }
                sendMessage();
            });
            function sendMessage(){
                if($("#msg").val()!==''){
                    // 서버로 문자 보내기
                    socket.emit('message', $("#msg").val());
                    $("#msg").val('');
                    $("#msg").focus();
                }
            }
        });
    </script>
</head>
<body>
    <div id="roomDiv">
        <h1>방만들기</h1>
        방이름 : <input type="text" name="roomName" id="roomName"/>
        <input type="button" value="방만들기" id="createRoom"/>
        <hr/>
    </div>
    <div id="showMessage">
        <h1>채팅하기</h1>
        Mesaage : <input type="text" name="msg" id="msg"/>
        <input type="button" value="보내기" id="sendMsg"/>
        <hr/>
    </div>
    <div>
        <h1>채팅내용보기</h1>
        <ul id="msgView" style="background-color:pink;">
        </ul>
    </div>
</body>
</html>
```



#### node18_socketio_room.js

```js
var http = require('http');
var fs = require('fs');

var server = http.createServer(function(request, response){
    fs.readFile(__dirname+'/chat_form_room.html', 'utf-8', function(error, htmlCode){
        if(!error){
            response.writeHead(200, {'Content-Type': 'text/html; charset=utf-8'});
            response.end(htmlCode);
        }else{
            response.writeHead(200, {'Content-Type': 'text/html; charset=utf-8'});
            response.end("존재하지 않는 페이지입니디.");
        }
    });
});

// 서버접속대기
server.listen(10040, function(){
    console.log("서버시작...    http://127.0.0.1:10040");
});

// 채팅 //////////////////////////////////////////////////////////////////
var socketio = require('socket.io');
var io = socketio.listen(server);

// 연결대기
io.sockets.on('connection', function(socket){
    // join 이벤트 발생시 실행할 대기
    var roomName; // 룸이름 보관
    socket.on('join', function(roomNameReceive){
        roomName = roomNameReceive;
        // 방이름을 socket 객체에 join 해주면 된다.
        socket.join(roomName);
    });

    // 메세지 주고 받기
    socket.on('message', function(msg){
        // 같은 방의 접속자에게 문자 보내기
        io.sockets.in(roomName).emit('response', roomName + '-->' + msg);
    });
});
```

