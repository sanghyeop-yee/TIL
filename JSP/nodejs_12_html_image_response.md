# Node.js 12: HTML Image Response

> Wed Aug 31, 2022

---



노드에서 html 파일 보내는 방법을 알아봅시다.

여러번 호출되고 있구나 알 수 있지요.

Html 문서한번 images 네번 총 다섯번이 



터미널창에 `npm install mime@2.5.2` 을 입력하면 node_modules 폴더가 새로 생깁니다. Node Package Manager 입니다. 



#### node12_html_image_response.js

```js
var http = require('http');
var fs = require('fs');

// 모둘추가 하는 방법 : npm // 터미널창에 npm install mime@2.5.2 입력
// 해당 프로젝트 위치에서 npm 을 이용하여 모듈을 추가한다.
//  >npm install mime@2.5.2
// mime 을 사용하기 위해 객체 생성
var mime = require('Mime');

var server = http.createServer(function(request, response){
    var url = request.url;
    console.log("url==>" + url);
    if(url == "/"){
        // index.html 파일을 비동기식으로 읽어 response 에 쓰기
        fs.readFile(__dirname+"/html/index.html", 'utf-8', function(e, htmlData){
            if(e){ // 파일읽기 실패
                response.writeHead(200, {"Content-Type":"text/html; charset=utf-8"});
                response.end("파일이 존재하지 않습니다.");
            }else{ // 파일읽기 성공
                response.writeHead(200, {"Content-Type":"text/html; charset=utf-8"});
                response.end(htmlData);
            }
        });
    }else if(url == "/subpage"){ // 서브페이지
        fs.readFile(__dirname+'/html/subpage.html', 'utf-8', function(err, data){
            if(!err){ // 성공
                response.writeHead(200, {"Content-Type":"text/html; charset=utf-8"});
                response.end(data);
            }else{ // 읽기실패
                response.writeHead(200, {"Content-Type": "text/html; charset=utf-8"});
                response.end("404 존재하지않는 페이지 입니다.");

            }
        });
        
    }else if(url.indexOf("/images") == 0){ // 이미지 요청일때 
        //  /images/bts2.gif
        var resource = url.substring(1);

        // 마임구하기
        var mimeName = mime.getType(resource);
        console.log("mimeName->" + mimeName);

        if(mimeName == null) mimeName="image/png";

        // 파일 읽어 response 에 쓰기를 하면 이미지가 클라이언트에게 전송된다.
        // c://node_test_2022/images...
        fs.readFile(__dirname+url, function(error, imgData){
            if(!error){ // 에러일때
                response.writeHead(200, {"Content-Type":mimeName});
                response.end(imgData);
            }
        });
    }
});

server.listen(10006, function(){
    console.log("server start...http://localhost:10006/")
});
```



#### index.html

```html
<!DOCTYPE html>
<html>
    <head>
        <title>노드에서 html 파일 보내기</title>
        <style>
            ul{
                list-style-type: none;
            }
            h1{
                text-align: center;
                background: #ddd;
            }
            img{
                width:200px;
                height:200px;
            }
        </style>
    </head>
    <body>
        <h1>California</h1>
        <ul>
            <li><a href="http://localhost:10006/subpage">서브페이지로 이동</a></li>
        </ul>
        <img src="./images/9202.png"/>
        <img src="./images/9204.png"/>
        <img src="./images/9205.png"/>
        <img src="./images/9206.png"/>
    </body>
</html>
```



#### subpage.html

```html
<!DOCTYPE html>
<html>
    <head>
        <title>노드 서브페이지</title>
    </head>
    <body>
        <h1>노드 서브페이지</h1>
        <img src="./images/9202.png"/>
    </body>
</html>
```

