# JavaScript: Regular Expression

> Web Jul 13, 2022

---

[toc]

로그인 페이지 등을 구현할 Form 내에서 사용될 regular expression 에 대해 배워봅니다.



```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style>
	ul, li{
		margin:0; padding:0; list-style-type:none;
	}
	li{
		float:left; height:80px; line-height:40px; border-bottom:1px solid #ddd;
	}
	li:nth-child(2n+1){
		width:30%;
	}
	li:nth-child(2n){
		width:70%;
	}
	li:last-child{
		width:100%; text-align:center;
	}
</style>
<script>
	function regExpTest(){
		var data = "Javascript and html5 and css3";
		// 정규식
		var reg = /Html5/i; // i: 대소문자를 구별하지 않음
		var result = reg.test(data);
		console.log(result);
		
		// 아이디 유효성 검사
		// ^ : 처음부터 검사, $ : 마지막까지 검사
		// 8~12 글자 사이여야 한다. 첫번째는 영어야 하며 숫자가 올 수 없다. 영어, 숫자, 특수문자는 $,_ 만 허용한다.
		
		reg = /^[A-Za-z]{1}[A-Za-z0-9_$]{7,11}$/;
		var userid = document.getElementById("userid").value;
		var id = document.getElementById("useridResult");
		if(!reg.test(userid)){
			id.innerHTML = "사용불가능한 아이디입니다.";
			id.style.color = "red";
			return false;
		}else{
			id.innerHTML = "사용가능한 아이디입니다.";
			id.style.color = "green";
		}
		// 이름 : 2~10글자, 한굴
		reg = /^[가-힣]{2,10}$/;
		if(!reg.test(document.getElementById("username").value)){
			alert("사용불가능한 이름입니다.");
			return false;
		}
		// 주민번호 뒷자리
		reg = /^[0-9]{2}[01]{1}[0-9]{1}[0-3]{1}[0-9]{1}-[1-8]{1}[0-9]{6}$/
		var jumin = document.getElementById("jumin1").value + "-" + document.getElementById("jumin2").value;
		if(reg.test(jumin)){
			alert("주민번호가 잘못되었습니다.");
			return false;
		}
		// 이메일
		reg = /^\w{8,20}[@][a-z0-9]{2,10}[.][a-z]{2,4}([.][a-z]{2,4})?$/
		if(!reg.test(document.getElementById("email").value)){
			alert("이메일을 잘못입력하였습니다.");
			return false;
		}
		// 연락처
		var tel = document.getElementById("tel1").value + "-";
			tel += document.getElementById("tel2").value + "-";
			tel += document.getElementById("tel3").value;
		// () : 1개는 반드시 있어야 한다.
		reg = /^(010|02|031|032|041|042)[-][0-9]{3,4}[-][0-9]{4}$/;
		if(!reg.test(tel)){
			alert("전화번호가 잘못입력되었습니다.");
			return false;
		}
		
		return true;
		
	}
	// 커서 옮기기
	function cursorMove(){
		if(document.getElementById("jumin1").value.length==6){
			document.getElementById("jumin2").focus();
		}
	}
</script>
</head>
<body>
	<h1> 정규표현식을 이용한 유효성검사 </h1>
	<form method="post" action="test.jsp" onsubmit="return regExpTest()">
		<ul>
			<li>아이디</li>
			<li><input type="text" name="userid" id="userid"/><span id="useridResult"></span></li>
			<li>이름</li>
			<li><input type="text" name="username" id="username"/></li>
			<li>주민번호</li>
			<li><input type="text" name="jumin1" id="jumin1" size="6" onkeyup="cursorMove()" value="840615"/>-
				<input type="text" name="jumin2" id="jumin2" size="6" value="3526847"/></li>
			<li>이메일</li>
			<li><input type="text" name="email" id="email"/></li>
			<li>연락처</li>
			<li>
				<input type="text" name="tel1" id="tel1" size="4"/> -
				<input type="text" name="tel2" id="tel2" size="4"/> -
				<input type="text" name="tel3" id="tel3" size="4"/>
			</li>
			<li><input type="submit" value="등록"/></li>
		</ul>
	</form>

</body>
</html>
```

