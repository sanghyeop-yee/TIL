# JavaScript: Array

> Mon Jul 11, 2022

---

[toc]

### 배열의 선언

* 배열의 크기를 지정하지 않아도 됩니다.
* 데이터형 다른 데이터를 하나의 배열에 보관할 수 있습니다.

```javascript
	var arr = new Array();
	
	arr[2] = 150;
	arr[5] = "John";
	arr[10] = "12.54";
	document.write("arr="+arr+"<br/>");
	
	var arr2 = new Array(1111, 2222, 3333, 'aaaa', 'bbbb');
	document.write("arr2="+arr2+"<br/>");
	
	var arr3 = [23, 43, 'abc', 'def'];
	document.write("arr3="+arr3+"<br/>");
```



### 배열에 데이터 추가하기

 배열에 데이터 추가하기 : 마지막에 값추가

```javascript
	arr2.push("570 21st Street, Oakland");
	document.write("arr2->"+arr2+"<br/>");
```



### join : 문자연결하기

```javascript
	arr2.push("570 21st Street, Oakland");
	document.write("arr2->"+arr2+"<br/>");
```



### pop : 마지막 데이터 삭제하기

```javascript
	arr2.pop();
	document.write("arr2.pop()->"+ arr2+ "<br/>");
```



### 배열의 이동

* `shift()` : 오른쪽데이터를 왼쪽으로 이동하고 0번째 index 의 값은 지워집니다.
* `unshift() `: 0번째 index 로 위치 시키고 하나씩 밀어냅니다.

```javascript
	arr2.shift(); // 1111, 2222, 3333, aaaa, bbbb ->  2222, 3333, aaaa, bbbb
	document.write("arr2.shift->"+ arr2+ "<br/>");
```

```javascript
arr2.unshift("570 21st Street, Oakland"); // 570 21st Street, Oakland, 2222, 3333, aaaa, bbbb ->  2222, 3333, aaaa, bbbb
	document.write("arr2.unshift()->"+ arr2+ "<br/>");
```



### JSON (JavaScript Object Notation) 데이터 처리하기

예를 들어 결제정보를 서버로 보낼때처럼 데이터를 주고받을때 사용합니다. 

```javascript
	var jData = {username : "John", 
				 tel: "650-040-4999", 
				 email:{
						firstemail:'johnjohn@gmail.com',
						secondemail:'johnsecond@gmail.com'
						},
				 gugudan : function(dan){
								var r="";
								for(var i=2; i<=9; i++){
									r += dan+"*"+i+"="+ (dan*i) + "<br/>";
								}
								return r;
						}
					};
	// JSON 데이터 접근하기
	document.write("username->"+jData.username+"<br/>");
	document.write("tel->"+jData.tel+"<br/>");
	document.write("email->"+jData.email.firstemail, jData.email.secondemail+"<br/>");
	document.write("gugudan-><br/>"+ jData.gugudan(3));
```



### 배열의 정렬

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
	function arraySortTest(){
		var numData = [36,43,74,86,23,-89,7,80];
		var txtData = ['tomato','apple','pineapple','peach','banana','orange','melon'];
		
		// Ascending Sort
		// 문자에 대한 오름차순
		txtData.sort();
		document.getElementById("result").innerHTML = "txtData asc->"+ txtData;
		// 숫자에 대한 오름차순
		numData.sort(function(a,b){return a-b;});
		document.getElementById("result2").innerHTML = "numData asc->"+ numData;
		// Descending Sort
		// 문자에 대한 내림차순
		// 오름차순으로 정렬후 역순으로 변경
		txtData.reverse();
		document.getElementById("result3").innerHTML = "txtData desc->"+ txtData;
		
		// 숫자에 대한 내림차순
		numData.sort(function(a, b){return b-a;});
		document.getElementById("result4").innerHTML = "numData desc->" + numData;
	}

</script>
</head>
<!-- body 가 로딩이 끝나면 호출되는 이벤트  -->
<body onload="arraySortTest();">
<h1>배열의 정렬</h1>
<div id="result"></div>
<div id="result2"></div>
<div id="result3"></div>
<div id="result4"></div>
</body>
</html>
```



