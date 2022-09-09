# Node.js 14: MySQL JDBC

> Fri Sep 2, 2022

---

[toc]

Node.js 에서 데이터베이스와 연결하는 방법을 알아봅시다.

Express 모듈을 추가 설치 합니다.



`npm install express`

`var express = require('express')`



mysqljdbc 라는 폴더를 생성하고 home.ejs, loginForm.html, MainProcess.js 파일을 생성합니다.



#### home.ejs

```ejs
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <!--
        ejs 파일 : embedded javascript template

        EJS 는 nodejs 에서 데이터를 html 에 바로 표시하고 싶을때 사용한다.
    -->
</head>
<body>
    <a href="/login">로그인</a>
    <a href="/logout">로그아웃</a>
    <a href="/board">게시판</a>
</body>
</html>
```



#### MainProcess.js

```js
// express 모듈 추가 설치
// cd mysqljdbc > npm install express

var http = require('http');
var express = require('express');
var fs = require('fs');

var app = express();
var server = http.createServer(app);
// 접속받기 *********************************************************
//      주소, 콜백함수
app.get('/home', function(request, response){ // 주소가 /home 이면 
    fs.readFile(__dirname+'/home.ejs', 'utf-8', function(error, homeData){
        if(!error){
            response.writeHead(200, {'Content-Type': 'text/html'});
            response.end(homeData);

        }
    });
});
// 로그인 폼
app.get('/login', function(request, response){
    fs.readFile(__dirname + "/loginForm.html", "utf-8", function(error, loginFormData){
        if(!error){
            response.writeHead(200, {'Content-Type': 'text/html'});
            response.end(loginFormData);
        }
    });
});


// 접속대기 *********************************************************
server.listen(10020, function(){
    console.log("express server start... http://localhost:10020/home");
});

```





#### loginForm.html

로그인 유효성 검사는 ajax 를 사용하겠습니다. jquery 사이트에 가서 cdn 주소를 가져옵니다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>로그인</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.1/jquery.min.js"></script>
    <script>
        $(function(){
            $("#logFrm").submit(function(){
                if($("#userid").val()==""){
                    alert("아이디를 입력하세요...");
                    $("#userid").focus();
                    return false;
                }
                if($("#userpwd").val()==""){
                    alert("비밀번호를 입력하세요...");
                    $("#userpwd").focus();
                    return false;
                }
                return true;
            })
        })
    </script>
</head>
<body>
    <h1>로그인</h1>
    <form method="post" action="/loginOk" id="logFrm">
        아이디 : <input type="text" name="userid" id="userid"/><br/>
        비밀번호 : <input type="password" name="userpwd" id="userpwd"/><br/>
        <input type="submit" value="login"/>
    </form>
</body>
</html>
```



여기까지 작성했으면 서버를 실행해서 확인해봅니다.



#### MainProcess.js

DB 연결을 위해 추가로 작성해줍니다.

```js
// express 모듈 추가 설치
// cd mysqljdbc > npm install express

var http = require('http');
var express = require('express');
var fs = require('fs');

var app = express();
var server = http.createServer(app);

// == post 방식의 데이터 request를 위한 설정
var bodyParser = require('body-parser');
app.use(bodyParser.urlencoded({extended:true}));

// == JDBC =======================================================
// mysql2 모듈이 필요하다.      > npm install mysql2

// ** 접속받기 ******************************************************
//      주소, 콜백함수
app.get('/home', function(request, response){ // 주소가 /home 이면 
    fs.readFile(__dirname+'/home.ejs', 'utf-8', function(error, homeData){
        if(!error){
            response.writeHead(200, {'Content-Type': 'text/html'});
            response.end(homeData);

        }
    });
});
// 로그인 폼
app.get('/login', function(request, response){
    fs.readFile(__dirname + "/loginForm.html", "utf-8", function(error, loginFormData){
        if(!error){
            response.writeHead(200, {'Content-Type': 'text/html'});
            response.end(loginFormData);
        }
    });
});
// 로그인 DB
app.post('/loginOk', function(request, response){
    var userid = request.body.userid;
    var userpwd = request.body.userpwd;
    console.log('아이디=', userid);
    console.log('비밀번호=', userpwd);
});


// ** 접속대기 ******************************************************
server.listen(10020, function(){
    console.log("express server start... http://localhost:10020/home");
});

```



`npm install mysql2` 로 MySQL 모듈을 다운받습니다.



#### MainProcess.js

```js
// express 모듈 추가 설치
// cd mysqljdbc > npm install express

var http = require('http');
var express = require('express');
var fs = require('fs');

var app = express();
var server = http.createServer(app);

// == post 방식의 데이터 request를 위한 설정
var bodyParser = require('body-parser');
app.use(bodyParser.urlencoded({extended:true}));

// == JDBC =======================================================
// mysql2 모듈이 필요하다.      > npm install mysql2
var mysqldb = require('mysql2');
mysqldb.autoCommit = true;

// DB 연결
var connection = mysqldb.createConnection({
    host: 'localhost',
    port: 3306,
    user: 'root',
    password: 'root1234',
    database: 'mydb'
});
connection.connect(); // -- DB가 연결됨.

// ** 접속받기 ******************************************************
//      주소, 콜백함수
app.get('/home', function(request, response){ // 주소가 /home 이면 
    fs.readFile(__dirname+'/home.ejs', 'utf-8', function(error, homeData){
        if(!error){
            response.writeHead(200, {'Content-Type': 'text/html'});
            response.end(homeData);

        }
    });
});
// 로그인 폼
app.get('/login', function(request, response){
    fs.readFile(__dirname + "/loginForm.html", "utf-8", function(error, loginFormData){
        if(!error){
            response.writeHead(200, {'Content-Type': 'text/html'});
            response.end(loginFormData);
        }
    });
});
// 로그인 DB
app.post('/loginOk', function(request, response){
    var userid = request.body.userid;
    var userpwd = request.body.userpwd;
    console.log('아이디=', userid);
    console.log('비밀번호=', userpwd);

    // 쿼리문 작성
    var sql = "select userid, username from member where userid=? and userpwd=?";
    //              쿼리문, 바인딩데이터        , 쿼리문을 실행후 콜백함수
    connection.query(sql, [userid, userpwd], function(error, results){ // results: 쿼리문 실행결과
        console.log(results);
    });
});


// ** 접속대기 ******************************************************
server.listen(10020, function(){
    console.log("express server start... http://localhost:10020/home");
});

```



