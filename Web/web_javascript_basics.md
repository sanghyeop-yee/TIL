# JavaScript : Basics

> Fri Jul 8, 2022

---

[toc]

자바스크립트의 영역은 Window, History, 등등이 있으며

Document 는 body tag 와 동일하다 보셔도 무방합니다.







### Function

함수는 호출하여야 실행됩니다. 아래의 Syntax 를 가집니다.

```javascript
/*			함수명([매개변수, 매개변수,....]) */
function test(){
	for(var i=2; i<=9; i++){
		r += "<li>"+7+"*"+i+"="+(7*i)+"</li>";
	}
  r += "</ul>";
  document.write(r);
}
```

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
/* 	함수 생성하기
	함수는 호출하여야 실행됨 */
	/*			함수명([매개변수, 매개변수,....]) */
	function gugudan(){
		var dan = document.getElementById("dan").value;
		if(!isNaN(dan)){ // 숫자가 아닐때 true, 숫자일때 false
		
			var r = "<ul>";
			for(var i=2; i<=9; i++){
				r += "<li>"+dan+"*"+i+"="+(dan*i)+"</li>";
			}
			r += "</ul>";
			document.getElementById("result").innerHTML=r;
		}else{
			document.getElementById("result").innerHTML = "Not a number";
		}
	}
	var hap = function(a, b){
		var h = a+b;
		document.getElementById("result").innerHTML=h;
	}
	var cha = (a, b)=>{
		var c = a - b;
		console.log(c);
	}
	
	// 함수호출
	// gugudan(5);
</script>
</head>
<body>
<input type="text" id="dan"/>
<input type="button" value="구구단" onclick="gugudan()"/>
<input type="button" value="합" onclick="hap(10,20)"/>
<input type="button" value="차" onclick="cha(10,20)"/>
<br/>
<div id="result"></div>

</body>
</html>
```



### String

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
	function stringTest(){
		var str = "자바스크립트 문자열 처리 함수";
		
		var tag;
		
		tag += "length="+ str.length;
		tag += "<br/>indexOf="+ str.indexOf("크");
		tag += "<br/>search="+ str.search("크");
		
		// slice(index, index), substring(index, index), substr(index, count)
		tag += "<br/>slice="+ str.slice(2, 8);
		tag += "<br/>substring="+ str.substring(2, 8);
		tag += "<br/>substr=" + str.substr(2, 8);
		
		tag += "<br/>replace=" + str.replace("스크립트", "script");
		
		var str2 = "Hello! JavaScript...";
		tag += "<br/>toUpperCase="+ str2.toUpperCase(str2);
		tag += "<br/>toLowerCase="+ str2.toLowerCase(str2);
		
		tag += "<br/>concat="+ str.concat(str2);
		tag += "<br/>charAt="+ str2.charAt(3);
		tag += "<br/>charCodeAt="+ str2.charCodeAt(3);
		
		var txtArr = str2.split("a");
		for(var idx = 0; idx<txtArr.length; idx++){
			tag += "<br/>txtArr["+idx+"]="+ txtArr[idx];
		}
		document.getElementById("view").innerHTML=tag; // outerHTML
		
	}
</script>

</head>
<body>
<div id="view"></div>
</body>
</html>
<script>
	stringTest();
</script>
```



### Math

```javascript
```



