# Node.js 08 : File Read

> Tue Aug 30, 2022

---

[toc]



> fs : FileSystem 파일 읽기/쓰기를 처리하는 모듈



파일을 읽기/쓰기를 하기 위해서는 절대주소가 필요합니다.

[[[ 노드 전체 영역의 전역변수(내장) ]]]

[1] __filename : 현재 실행중인 파일의 절대경로와 파일명

[2] __dirname : 현재 실행중인 파일의 절대경로를 가진다. 



파일을 읽고 쓰는 방법은 동기식과 비동기식이 있습니다.

### 동기식

```js
// fs : FileSystem 파일 읽기/쓰기를 처리하는 모듈
var fs = require('fs');

// 파일을 읽기/쓰기를 하기 위해서는 절대주소가 필요하다.
// [[[ 노드 전체 영역의 전역변수(내장) ]]]
// [1] __filename : 현재 실행중인 파일의 절대경로와 파일명
// [2] __dirname : 현재 실행중인 파일의 절대경로를 가진다. 

// 파일을 읽고 쓰는 방법은 동기식과 비동기식이 있다.
// 동기식 : 명령이 바로 실행하는 것.
// 비동기식 : 스레드에 의해서 실행된다. 

// 1. 비동기식 파일 읽는 방법
//          파일경로    +   파일명                  ,  인코딩  , 콜백함수(에러정보, 읽은내용)
fs.readFile(__dirname+ '/dataFile/file_read.txt', 'utf-8', function(error, data){
    // 파일의 내용을 읽은 후 실행되는 콜백함수
    if(error){ // 읽기 에러 발생시 error 에 정보가 있으면 true, error 에 정보가 없으면 false
        console.log("파일 읽기 실패하였습니다.");
        console.log(error);
    }else{ // 읽기성공
        console.log("파일명 : /dataFile/file_read.txt를 비동기식으로 읽기");
        console.log(data);
    }
});
```





### 비동기식

```ㅌ
// fs : FileSystem 파일 읽기/쓰기를 처리하는 모듈
var fs = require('fs');

// 파일을 읽기/쓰기를 하기 위해서는 절대주소가 필요하다.
// [[[ 노드 전체 영역의 전역변수(내장) ]]]
// [1] __filename : 현재 실행중인 파일의 절대경로와 파일명
// [2] __dirname : 현재 실행중인 파일의 절대경로를 가진다. 

// 파일을 읽고 쓰는 방법은 동기식과 비동기식이 있다.
// 동기식 : 명령이 바로 실행하는 것.
// 비동기식 : 스레드에 의해서 실행된다. 

// 1. 비동기식 파일 읽는 방법 (readFile 함수 사용)
//          파일경로    +   파일명                  ,  인코딩  , 콜백함수(에러정보, 읽은내용)
fs.readFile(__dirname+ '/dataFile/file_read.txt', 'utf-8', function(error, data){
    // 파일의 내용을 읽은 후 실행되는 콜백함수
    if(error){ // 읽기 에러 발생시 error 에 정보가 있으면 true, error 에 정보가 없으면 false
        console.log("파일 읽기 실패하였습니다.");
        console.log(error);
    }else{ // 읽기성공
        console.log("파일명 1 : /dataFile/file_read.txt를 비동기식으로 읽기");
        console.log(data);
    }
});

// 2. 동기식 파일 읽는 방법 (readFileSync 함수 사용)
try{
    var data = fs.readFileSync(__dirname+'/node01_start.js', 'utf-8');
    console.log("파일명 2 : /node01_start.js 를 동기식으로 읽기=========");
    console.log(data);
}catch(error){
    console.log("동기식 파일일기 예외 발생..."+ error);
}

```

