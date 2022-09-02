# Node.js 11: File Append

> Wed Aug 31, 2022

---



a 텍스트 파일의 내용을 읽어서 b 텍스트 파일에 추가하는 방법을 알아봅시다.



```js
// 파일의 원 내용을 유지하고 새로운 내용을 추가하기

var fs = require('fs');

// 비동기식으로 파일 추가
var appendData = "\n----비동기식으로 파일 내용 추가하기 테스트 중............."
fs.appendFile(__dirname+"/dataFile/txtWrite.txt", appendData, 'utf-8', function(err){
    if(!err){
        console.log("파일내용이 정상 추가 되었습니다..");
    }else{
        console.log("파일 내용 추가 실패하였습니다.");
    }
});

// 동기식으로 파일 추가
var appData = "\n재규어 랜드로버 코리아(대표 로빈 콜건)가 23일 강원도 홍천군에서 랜드로버의 럭셔리 플래그십 SUV 올 뉴 레인지로버의 공식 출시 행사를 갖고 본격적인 판매에 돌입한다.";
try{
    // dataFile/file_read.txt 읽기한 후 읽은 내용을 txtWrite.txt 에 추가하기
    fs.readFile(__dirname+"/dataFile/file_read.txt", 'utf-8', function(err, data){
        if(err){
            console.log("read failed...");
        }else{
            fs.appendFileSync(__dirname+"/dataFile/txtWrite.txt", "\n"+data, 'utf-8');
            console.log("동기식 파일내용 추가 완료됨...");
        }
    });
}catch(e){
    console.log("동기식 파일 추가 에러 발생...");
}

```

