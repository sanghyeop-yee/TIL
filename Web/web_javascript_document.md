# JavaScript: Document 객체

> Tue Jul 12, 2022

---

[toc]

Document 객체는 웹 페이지 그 자체를 의미합니다.

웹 페이지에 존재하는 HTML 요소에 접근하고자 할 때는 반드시 Document 객체부터 시작해야 합니다.



### Document 메소드

Document 객체는 HTML 요소와 관련된 작업을 도와주는 다양한 메소드를 제공합니다.

 \- HTML 요소의 선택

 \- HTML 요소의 생성

 \- HTML 이벤트 핸들러 추가

 \- HTML 객체의 선택



### HTML 요소의 선택

|                   메소드                    |                     설명                      |
| :-----------------------------------------: | :-------------------------------------------: |
|   document.getElementsByTagName(태그이름)   |     해당 태그 이름의 요소를 모두 선택함.      |
|       document.getElementById(아이디)       |         해당 아이디의 요소를 선택함.          |
| document.getElementsByClassName(클래스이름) |    해당 클래스에 속한 요소를 모두 선택함.     |
|   document.getElementsByName(name속성값)    | 해당 name 속성값을 가지는 요소를 모두 선택함. |
|      document.querySelectorAll(선택자)      |  해당 선택자로 선택되는 요소를 모두 선택함.   |





```javascript

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body link="red" alink="green" vlink="orange">
<a href="https://www.daum.net">다음홈페이지</a><br/>
<a href="https://www.nate.com">네이트홈페이지</a><br/>
<a href="https://www.jquery.com">jQuery홈페이지</a><br/>
<input type="text" name="userid" class="userid"/><br/>
<input type="text" name="username" id="username"/><br/>
<div style="background-color:#ddd;">
	<script>
		document.title = "자바스크립트에서 제목설정하기";
		document.write("location=>"+ document.location); // 프로토콜, url, port, 경로, 파일명
		document.write("<br/>URL=>"+ document.URL);
		document.write("<br/>domain=>"+ document.domain);
		document.write("<br/>lastModified=>"+ document.lastModified);
		
		document.bgColor= "gold";
		document.write("<br/>bgColor=>"+ document.bgColor);
		
		// 링크 컬러설정
		document.linkColor= "yellow";
		document.write("<br/>linkcolor=>"+ document.linkColor+ "<br/>alinkColor=>"+ document.alinkColor+"<br/>vlinkColor=>"+ document.vlinkColor);
		
		// 쿠키
		document.cookie = "값을 저장할 수 있다.";
		document.write("<br/>cookie=>", document.cookie);
		
		function domSelect(){
			document.getElementById("username").value="John";
			document.querySelector("#username").value="MJ";
			
			document.getElementsByTagName("a")[1].title = "nate.com 으로 이동합니다.";
			document.getElementsByClassName("userid")[0].value= "John Lee";
		}
		domSelect();
	</script>
</div>
</body>
</html>
```





### Cookie.html

cookie 데이터는 client 의 컴퓨터에 JavaScript 를 통해 기록되며, 서버에 Java 를 통해 기록되는것은 session 이라고 합니다. 

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
	function popup(){
		// 쿠키정보를 확인하여 팝업띄우기
		var cookie = document.cookie; //name=value; name=value
		if(cookie.indexOf("pop=yes")==-1){
			window.open("popup.html","w","width=400px, height=400px");	
		}
	}
</script>
</head>
<body onload="popup()">
<img src="../img/01.png"/>
</body>
</html>
```

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style>
	img{width:100%;}
</style>
<script>
	// close 선택시 호출할 함수
	function popupCookie(){
		// true : check 상태일때, false : check 안된상태일때
		var check = document.getElementById("chk").checked
		if(check){ // 체크상태일때
			// 쿠키를 기록한다. 
			var now = new Date(); // 오늘날짜와 시간 객체로 구하기
			//				Name=value;
			now.setDate(now.getDate()+1); // 하루지난 날짜 구하기
			var cookieData = "pop=yes;path=/;expires="+now;
			console.log(cookieData);
			document.cookie = cookieData;
			document.cookie = "notice=ok";
		}
		self.close(); // window.close();
	}
</script>
</head>
<body>
	<img src="../img/02.png"/>
	<div>
		<input type="checkbox" id="chk"/> 하루동안 안보기
		<button onclick="popupCookie()">Close</button>
	</div>
</body>
</html>
```

