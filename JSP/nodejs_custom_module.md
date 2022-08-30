# Node.js : Custom Module

> Tue Aug 30, 2022

---



> Node.js 에서 모듈이란 필요한 함수들의 집합을 의미합니다.



사용자 정의 모듈을 만들어 봅시다.

모듈을 생성하기 위해서는 exports 라는 내장객체를 이용하여 변수 또는 함수를 정의합니다.



#### node04_custom_module_create.js

```js
// 사용자 정의 모듈만들기
// 모듈을 생성하기 위해서는 exports 라는 내장객체를 이용하여 변수 또는 함수를 정의한다.

// 1. 변수 선언하는 방법
exports.num = 1234;
exports.name = "홍길동";

// 2. 함수를 모듈로 선언하는 방법
/* 
function hap(n1, n2){
   return n1+n2;
}
*/
exports.hap = function(n1, n2){
    return n1+n2;
}

// 임의의 값까지 합 구하기
exports.sum = function(max){
    var s=0;
    for(var i=1; i<=max; i++){
        s += i;
    }
    return s;
}

// 단을 입력하여 해당하는 구구단을 만드는 함수
exports.gugudan = (dan)=>{
    var result = "";
    for(var i=2; i<=9; i++){
        result += dan + " * " + i + " = " + (dan*i) + "<br/>";
    }
    return result;
}


```



#### node05_custom_module_test.js

```js
var http = require('http');
// 사용자 정의 모듈 사용하기
var my_module = require('./node04_custom_module_create')

var server = http.createServer(function(request, response){
    response.writeHead(200, {"Content-Type" : "text/html; charset=UTF-8"});
    response.write("번호 -->" + my_module.num);
    response.write("<br/>이름 -->"+ my_module.name);
    response.write("<br/>합(5,7) -->"+ my_module.hap(5, 7));
    response.write("<br/>sum(100) -->" + my_module.sum(100));
    response.end("<hr/>구구단(9)<br/>" + my_module.gugudan(9));

});

server.listen(10002, function(){
    console.log("server start...http://localhost:10002");
});
```



새로운 터미널을 열어 테스트를 해봅시다.

`node node5_custom_module_test.js`



```bash
sanghyeop/node_test_2022 - (master) > node node05_custom_module_test.js
server start...http://localhost:10002
```

