# JavaScript: Date/Time

> Mon Jul 11, 2022

---

[toc]

서버 과부하가 발생하지 않기위해 자바스크립트를 통해서 스스로의 컴퓨터내에서 시간/날짜를 관리하고 처리하는 방법을 배워봅시다.

날짜를 객체로 구합니다.



### Date/Time

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
	var setDateTime = function(){
		// 현재시스템의 날짜, 시간정보 얻어오기
		var now = new Date();
		
		document.getElementById("now").innerHTML = now;
		
		// 날짜 변경
		var now1 = new Date("2023.04.20");
		var now2 = new Date(4842328645); // 밀리초 1970년 1월 1일 0시 0분 0초 기준으로 계산된다.
		document.getElementById("result").innerHTML = now1 + "<br/>" + now2;
		
		// 년 : getFullYear(), 월 : getMonth(), 일 : getDate()
		// 시 : getHours(), 	  분: getMinutes(), 초 : getSeconds()
		// 요일 : getDay() -> 일요일: 0, 월요일: 1,,,,,,,
		
		var tag = now.getFullYear()+"-"+(now.getMonth()+1)+"-"+now.getDate();
		tag += " "+now.getHours()+":"+ now.getMinutes()+":"+now.getSeconds();
		tag += "("+ now.getDay()+")";
		
		document.querySelector("#view").innerHTML = tag;
	}
</script>
</head>
<body onload="setDateTime()">
<h1>날짜와 시간</h1>
<h1 id="now"></h1>
<div id="result"></div>
<div id="view"></div>
</body>
</html>
```



### Time Interval

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
	function setTimer(){
		var now = new Date();
		
		var hour = now.getHours();
		var minute = now.getMinutes();
		var second = now.getSeconds();
		
		var nowTxt = "";
		if(hour<10){
			nowTxt += "0";
		}
		nowTxt += hour + ":";
		
		if(minute<10){
			nowTxt += "0";
		}
		nowTxt += minute + ":";
		if(second<10){
			nowTxt += "0";
		}
		nowTxt += second;
		
		document.getElementById("clock").innerHTML = nowTxt;
	}
	var step=5;
	var left=0;
	function moveImage(){
		var img = document.getElementById("i").style;
		left = left + step;
		img.left = left+"px";
		if(left>500){
			step = -5;
		}
		if(left<0){
			step = 5;
		}
	}
	var interval;

</script>
<style>
	#i{width: 100px; height:100px; position:absolute; left:0px; top:0px;}
</style>
</head>
<!-- 설정한 시간 간격으로 호출됨 : setInterval("호출할함수명", 밀리초); -->
<!-- interval을 중지시킨다 : clearInterval() -->
<body onload="interval = setInterval('moveImage()', 500); setInterval('setTimer()', 1000);">
<img src="../img/02.png"/>
<h1 id="clock"></h1>
<img src="../img/01.JPG" id="i" onmouseover="clearInterval(interval)" 
								onmouseout="interval=setInterval('moveImage()', 100)"/>
</body>
</html>
```



### SetTimeout

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>

	var imgTag="";
	function addImage(){
		cnt++
		imgTag += "<img src='../img/02.png' width='100'/>"
		document.getElementById("imgView").innerHTML = imgTag;
		// 재귀호출
		setTimeout('addImage()', 1000);
		if(cnt>10){
			clearTimeout(timer); // 반복호출 중지
		}
	}
	
</script>
</head>
<body onload="addImage()">
<h1>setTimeout 을 이용한 반복실행</h1>
<div id="imgView"></div>
</body>
</html>
```

