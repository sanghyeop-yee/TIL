# Node.js : Custom Event

> Tue Aug 30, 2022

---





```js
var http = require('http');
//--- 이벤트 발생시 처리할 함수------------
// on, addListener(), once()
// 1. 이벤트가 정의되어 있는 events 모듈을 객체화 하여야 한다.
var events = require('events');

// 2. 이벤트 처리를 하기 위해서는 모듈을 담고 있는 객체 (EventEmitter) 를 초기화 하여야 한다.
var event_object = new events.EventEmitter();
var cnt = 1;
// 3. 이벤트 처리 방법 1
//              이벤트종류, 실행할 함수
/*
event_object.on('call', ()=>{ // 2번 호출됨
    console.log("on() 을 이용한 이벤트 처리함..." + cnt++);
});
*/

// 4. 이벤트 처리 방법 2
/*
event_object.addListener('call', function(){ // 2번 호출됨
    console.log('addListener()을 이용한 이벤트 처리됨...'+ cnt++);
});
*/

// 5. 이벤트 처리 방법 3
event_object.once('call', function(){
    console.log("once()을 이용한 이벤트 처리됨..." + cnt++);
});

//-----------------------------------
var server = http.createServer(function(request, response){

    // 이벤트 발생
    //               이벤트종류
    event_object.emit('call'); // 강제로 이벤트 발생 시킴

    response.writeHead(200, {"Content-Type":"text/html; charset=UTF-8"});
    response.end("<h1>사용자 이벤트 테스트 중</h1>");
});

server.listen(10003, function(){
    console.log("server start... http://localhost:10003");
});
```

