# JavaScript: Event

> Wed Jul 13, 2022

---

[toc]

클릭을 하거나 마우스를 이동했을때 이벤트를 발생시킬수 있습니다.

submit 이 발생하면 onsubmit 이벤트가 발생합니다.

form 을 이용하여 input 으로 submit 을 받거나 button 을 자바스크립트로 받을수 있습니다. 



### javascript의 이벤트 

​	onclick : 마우스 클릭하면



​	onmouseover : 마우스 객체에 들어가면 - 하위객체도 포함 

​	onmouseout : 마우스 객체에서 나오면 - 하위객체도 포함

​	

​	onmouseenter : 마우스가 객체에 들어오면

​	onmouseleave : 마우스가 객체에서 나가면

​	

​	onmousedown : 마우스를 누른상태

​	onmouseup : 마우스를 누른 후 놓으면

​	onmousemove : 마우스를 움직이면

​	

​	onfoucs : 컴퍼넌트에 커서가 들어가면

​	onblur : 컴퍼넌트에서 커서가 나가면

​	

​	onkeydown : 키를 누른 상태 onkeyrelease

​	onkeyup : 키를 누른 후 놓으면 onkeypress



```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style>
	#t1, #t3{
		height:100px;
		padding:50px;
		background-color:pink;
	}
	#t1>h1{
		background:yellow;
		height:80px;
	}
	#t1 b{
		background:orange;
		height:50px;
	}
	#t2{
		height:100px;
		padding:50px;
		background-color:blue;
	}
	#t2>h1{
		background:lightblue;
		height:80px;
	}
	#t2 b{
		background:#ddd;
		height:50px;
	}
	
	/*로그인*/
	ul, li{margin:0; padding:0; list-style-type:none;}
	#login{
		width:400px; margin:0 auto; padding:50px; overflow:auto; border:1px solid gray;
	}
	#login li{
		float: left;
		height:50px;
	}
	#login li:nth-child(2n+1){
		width:20%;
	}
	#login li:nth-child(2n){
		width:80%;
	}
	#login li:last-child{
		width:100%;
		text-align: center;
	}
</style>
<script>
	// javascript의 이벤트 
	/*
	onclick : 마우스 클릭하면
	
	onmouseover : 마우스 객체에 들어가면 - 하위객체도 포함 
	onmouseout : 마우스 객체에서 나오면 - 하위객체도 포함
	
	onmouseenter : 마우스가 객체에 들어오면
	onmouseleave : 마우스가 객체에서 나가면
	
	onmousedown : 마우스를 누른상태
	onmouseup : 마우스를 누른 후 놓으면
	onmousemove : 마우스를 움직이면
	
	onfoucs : 컴퍼넌트에 커서가 들어가면
	onblur : 컴퍼넌트에서 커서가 나가면
	
	onkeydown : 키를 누른 상태 onkeyrelease
	onkeyup : 키를 누른 후 놓으면 onkeypress
	
	*/
	var cnt=0, cnt2=0;
	function mouseoverTest(){
		document.getElementById("view").innerHTML = "onmouseover이벤트가 "+ ++cnt +"번째 발생하였습니다.";
	}
	function mouseoutTest(){
		document.getElementById("view2").innerHTML = "onmouseout이벤트가 "+ ++cnt +"번째 발생하였습니다.";
	}
	var c=0, c2=0;
	function mouseenterTest(){
		document.getElementById("view").innerHTML = "onmouseenter이벤트가 <b>"+ ++c +"번째 </b> 발생하였습니다.";
	}
	function mouseleaveTest(){
		document.getElementById("view2").innerHTML = "onmouseleave이벤트가 <b>"+ ++c2 +"번째 </b> 발생하였습니다.";
	}
	function mouseEvent(){
		document.getElementById("view").innerHTML = "<h1>"+ a +"</h1>";
	}
	function setBackPink(obj){
		obj.style.backgroundColor="pink";
	}
	function setBackBlue(obj){
		obj.style.backgroundColor="lightblue";
	}
	// 아이디와 비밀번호 입력을 입력여부 확인 함수
	function loginCheck(){
		// 
		var userid = document.getElementById("userid");
		if(userid.value==""){
			alert("아이디를 입력하세요.");
			userid.focus();
			return false;
		}
		var userpwd = document.getElementById("userpwd");
		if(userpwd.value==""){
			alert("비밀번호를 입력하세요.");
			userpwd.focus();
			return false;
		}
		// return true;
		// 아이디와 비밀번호가 있다. 
		// 자바스크립트로폼을 submit 을 발생시킨다. 
		document.getElementById("frm").submit();
	}
	function numKeyCheck(){
		// 	 event 내장객체
		console.log(event.keyCode);
		var code = event.keyCode;
		if(code>=48 && code<=57){ // 숫자
			event.returnValue = true;
		}else{ // 숫자 아닐때
			event.returnValue = false;
		}
	}
	
</script>
</head>
<body>
<div id="login">
	<!-- <form method="post" action="loginOk.jsp" onsubmit="return loginCheck()"> -->
	<form method="post" id="frm" action="loginOk.jsp">
		<h1>로그인</h1>
		<ul>
			<li>번호</li>
			<li><input type="text" name="num" id="num" onkeydown="numKeyCheck()"/></li>
			<li>아이디</li>
			<li><input type="text" name="userid" id="userid" onfocus="setBackPink(this)" onblur="setBackBlue(this)"
								   onblur="setBackBlue(this)" onchange="console.log(this.value+'로 변경되었습니다.');"/></li>
			<li>비밀번호</li>
			<li><input type="password" name="userpwd" id="userpwd" onfocus="setBackPink(this)" onblur="setBackBlue(this)"/></li>
			<li>
				<input type="submit" value="로그인" onfocus="setBackPink(this)" onblur="setBackBlue(this)"/>
				<button>로그인</button>
				<input type="image" src="../img/button.png"/>
				
				<input type="button" value="로그인" onclick="loginCheck()"/>
			</li>
		</ul>
	</form>
</div>

<div id="t1" onmouseover="mouseoverTest()" onmouseout="mouseoutTest()">
	<h1>onmouseover 이벤트, <b>onmouseout</b> 이벤트</h1>
</div>

<div id="t2" onmouseenter="mouseenterTest()" onmouseleave="mouseleaveTest()">
	<h1>onmouseenter 이벤트, <b>onmouseleave</b> 이벤트</h1>
</div>

<div id="t3" onmousedown="mouseEvent('down')" onmouseup="mouseEvent('up')" onmousemove="mouseEvent('move')"></div>
<div id="view"></div>
<div id="view2"></div>
</body>
</html>
```

