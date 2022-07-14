# JavaScript - jQuery: Basics

> Thu Jul 14, 2022

---

[toc]

jQuery 란?

> JavaScript 로 만들어진 이벤트 기반 라이브러리 입니다.



CDN 방식



기본코드를 템플릿이라고 합니다.

Preference > Web > HTML Files > Editor > Templates > New HTML File (5) > Edit 으로 이동하여 Title 밑에 아래의 코드를 붙여넣습니다.

`<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>`



### Start

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- 
	jquery : 이벤트 기반 자바스크립트 라이브러리이다.
	
	cdn 방식으로 링크를 걸어서 사용
	jquery.com -> 다운로드 -> CDN 링크로 이동하여 링크 복사
 -->
 <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
 <script>
 	var a;
 	// alert("aaaa");
 	function f(){
 		var b;
 	}
 	// jquery 시작 : document 가 로딩이 완료되면 ready 이벤트에 의해 jquery 가 실행된다.
 	// 선택자
 	/* 
 	$(document).ready(function(){
 		// 실행문;
 		alert("jquery 실행됨");
 	});
 	*/
 	/* 
 	jQuery(document).ready(function(){
 		alert("jQuery(document) 실행됨");
 	}); 
 	*/
 	$(function(){
 		alert("제이쿼리 실행됨...");
 	});
 	
 </script>
</head>
<body>
<h1>jquery 실행하기</h1>
<script>
	// alert("bbbb");
</script>
</body>
</html>
```



### CSS Attribute

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script>
	$(function(){
		// jquery
		// jquery 에서 스타일시트 사용하기
		// 태그 선택자  속성, 속성값   background-color, backgroundColor
		$("body").css("background-color", "#ddd");
		// 클래스 선택자
		$(".c1").css("border", "1px solid red");
		// 아이디 선택자
		$("#t1").css("background-color", "lightblue");
		
		// jquery 에서 html 속성 제어하기
		// 속성=속상값
		$("img").attr("src", "../img/02.png");
		$("#txt").attr("value", "John");
		
		// 속성만 있는 경우 readonly, checked, selected, disabled, controls...
		$("#txt").prop("readonly", true);
		
	});
</script>
</head>
<body>
<input type="text" id="txt"/><br/>
<h1 class="c1">JQuery를 이용한 클래스 선택자 사용하기</h1>
<h1 id="t1">JQuery를 이용한 아이디 선택자 사용하기</h1>
<h1 class="c1">JQuery를 이용한 태그 사용하기</h1>
<img src="../img/01.png"/>

</body>
</html>
```



### Select Adjoin : 선택자

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script>
	$(function(){
		// 인접선택자
		//	  모든선택자 (*), 태그선택자('img'), 아이디선택자('.i'), 클래스선택자('#i'), 자손선택자('a>b'), 후손선택자('a b')
		
		// children() : 자손을 선택
		$("div").children(); // 자손 모두
		$("div").children('.one').css('background', 'pink'); // 자손 중 class=one
		// next() : 선택자의 다음 객체를 선택한다. 
		$("#first").next().css("color", "red");
		// prev(); 선택자의 이전 객체를 선택한다.
		$("img").prev().css("border", "2px solid green").css("margin","50px");
		// parent() : 부모선택자
		$("img").parent().css("background", "lightblue");
		// siblings() : 형제 선택자
		$("h2").siblings().css("color", "blue");
		
		$("div").next().attr("title", "놀러가자~~");
	});
</script>
</head>
<body>
	<div>
		<h1 id="first">인접선택자</h1>
		<h1 class="one">children선택자</h1>
		<h2 class="one">next선택자</h2>
		<h4>prep선택자</h4>
		<h4>parent선택자</h4>
		<h3>sibling선택자</h3>
		<h2>후손, 자손선택자</h2>
	</div>
	<img src="../img/03.png"/>
</body>
</html>
```



### Select Position

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script>
	$(function(){
		// 위치선택자
		$("#menu>li:first").css('color','red'); // 첫번째 first, last, even, odd
		$("#menu>li:last").css('color', 'green');
		$("#menu>li:even").css("background","lightblue");
		$("#menu>li:odd").css("background", "pink");
		
		$("#menu>li:nth-last-of-type(3)").css("border", "2px solid red"); // 뒤에서 3번째 객체
		$("#menu>li:nth-child(3n)").css('font-size', '1.5em');
		
		// li 태그의 부모 중 자식이 1개인 li을 선택한다.
		$("li:only-child").css("background-color", "yellow");
		
		// index 를 이용한 선택
		$("#menu>li").slice(3).css('color', 'blue'); // index 3부터 끝까지 선택자
		$("#menu>li").eq(2).css('background', 'orange'); // index 가 2인 위치를 선택
		
		// lt(): less than, gt(): greater than
		$("#menu>li:lt(5)").css('font-size', "2em").css('color', 'cyan');
		$("#menu>li:gt(3)").css('color', '#f00');
	});
</script>
</head>
<body>
<ul id="menu">
	<li>html</li>
	<li>css</li>
	<li>javascript</li>
	<li>jquery</li>
	<li>ajax</li>
	<li>spring</li>
	<li>mybatis</li>
</ul>
<ul style="background:green">
	<li>위치선택자</li>
</ul>
</body>
</html>
```

