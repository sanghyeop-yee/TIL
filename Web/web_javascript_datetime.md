# JavaScript: Date/Time

> Mon Jul 11, 2022

---

[toc]

서버 과부하가 발생하지 않기위해 자바스크립트를 통해서 스스로의 컴퓨터내에서 시간/날짜를 관리하고 처리하는 방법을 배워봅시다.

날짜를 객체로 구합니다.



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

