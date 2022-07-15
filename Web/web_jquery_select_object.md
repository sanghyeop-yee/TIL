# JavaScript - jQuery: Select, Object

> Fri Jul 15, 2022

---

[toc]

### Select Attribute

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script>
	function jsFunction(){
		alert("클릭");
	}
	$(function(){
		// 속성선택자
		
		// A태그에 title속성이 있으면 선택
		$('a[title]').css("background-color", "yellow");
		
		// 속성값으로 선택하기
		$('a[href="https://www.nate.com"]').css("border", "2px solid red");
		
		// ^ : 특정한 값으로 시작하면
		$('a[href^="http://"]').css("padding", "20px").css("border", "1px solid blue");
		
		// $ : 특정값으로 끝나면
		$('a[href$="com"]').css("background-color", "orange");
		
		// * : 특정값이 포함되면
		$('a[href*="a"]').css("background-color", "green");
		
	});
</script>
</head>
<body>
<ul>
	<li><a href="https://www.daum.net">다음</a></li>
	<li><a href="https://www.nate.net">네이트</a></li>
	<li><a href="https://www.naver.net" title="nate홈으로 이동하기">네이버</a></li>
	<li><a href="http://www.android.org">안드로이드</a></li>
	<li><a href="mailto:aaaaa.nate.com">메일보내기</a></li>
	<li><a href="http://www.w3schools.com">w3school</a></li>
	<li><a href="javascript:jsFunction()">w3school</a></li>
	
	
</ul>

</body>
</html>
```



### Select State Content

```javascript
```



### Object Handler

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<style>
	.c1{border:2px solid red; width:200px; height:200px;}
	.c2{border:2px solid blue; width:200px; height:200px;}
	.c3{border:2px solid green; width:200px; height:200px;}
	.c4{border:2px solid orange; width:200px; height:200px;}
</style>
<script>
	$(function(){ // $(()=>{
		// html() : 특정객체에 html 태그를 추가한다.  => 자바스크립트의 innerHTML 와 유사하다
		var tag = "<a href=''>다음</a>을 클릭하시면 페이지가 이동합니다.";
		$('#result').html(tag);
		
		console.log($('#result').html());
		
		// text() : 특정객체에 문자를 추가한다. 
		var tag2 = "<div>text()속성 연습중</div>"
		$("#result").text(tag2);
		
		console.log( $('a').text() );
		
		$('h2').children('a').attr('href', 'https://www.nate.com');
		// 속성지우기
		$('a').removeAttr('href');
		$('h1').removeAttr('id');
		
		// 클래스 handler
		// style 시트의 클래스 조작하는 방법
		// addClass() : 클래스 추가
		$('#lst>img').addClass('c1');
		$('#lst>img').addClass('c2');
		
		// removeClass() : 클래스 삭제
		$("#lst>img").removeClass("c2");
		
		// 짝수번째는 : c3, 홀수번째는 : c4
		$("#lst>img:even").addClass('c3');
		$('#lst>img:odd').addClass('c4');
		
		// toggleClass() : 클래스 있으면 지우고 없으면 추가
		$('#lst>img').toggleClass('c3');
		
		// hasClass() : 클래스가 존재하는지 유무 확인 (true, false)
		var h = $("#lst>img:first").hasClass('c4');
		console.log(h)
		$('h1').html(h+"");
		
		// val() : 폼의 value 를 구하거나 셋팅한다 
		// html(), text(), attr(), prop()
		
		
	});
</script>
</head>
<body>
<div></div>
<h1 id="obj1">객체 조작 1</h1>
<h2><a href="">객체 조작 2</a></h2>
<div id="result"></div>
<div id="lst">
	<img src="../img/01.png"/>
	<img src="../img/02.png"/>
	<img src="../img/03.png"/>
	<img src="../img/04.png"/>
</div>
</body>
</html>
```



### Object 1

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<style>
	ul,li{margin:0; padding:0;}
	img{width:100px; height:100px;}
	li{list-style-type:none; float:left; border:1px solid red; margin:4px; padding:4px;}
</style>
<script>
	$(()=>{
		// before() : 선택자 이전에 객체를 추가
		$("#i").before("<li><img src='../img/02.png'/></li>");
		// insertBefore() : 선택자 이전에 객체를 추가
		// 내용			선택자
		$("<li><img src='../img/03.png'/></li>").insertBefore("#i");
		// after() : 선택자 다음에 객체를 추가
		$("#i").after("<li><img src='../img/04.png'/></li>");
		// insertAfter() : 선택자 다음에 객체를 추가
		$("<li id='copy'><img src='../img/sf01.jpeg'/></li>").insertAfter("#i")
		// append() : 선택자의 내용중에 제일 마지막에
		$('ul').append("<li><img src='../img/sf02.jpeg'/></li>")
		// appendTo() : 내용, 선택자
		$("<li><img src='../img/sf03.jpeg'/></li>").appendTo('ul');
		// html()	$('ul').html("<li><img src='../img/sf02.jpeg'/></li>")
		// prepend(), prependTo() :  선택한 요소내에 제일 처음에 추가
		$("ul").prepend("<li><img src='../img/sf04.jpeg'/></li>");
		$("<li><img src='../img/sf05.jpeg'/>").prependTo('ul');
		
		// clone() : 요소 복사하기
		var element = $("#copy").clone();
		element.attr("id", "copy2");
		$('ul').prepend(element);
		
		// empty() : 선택자의 내용을 지우기
		$("#copy").empty();
		
		// remove() : 선택자와 내용을 지우기
		$("li:first").remove();
		
		// replaceAll(), replaceWith()	: 선택자를 다른 객체로 치환
		// $("<h1>replaceAll</h1>").replaceAll("#copy");
		// $("#i").replaceWith("<h1>ReplaceWith</h1>");
		setInterval('imgMove()', 1000);
		
	});
	
	function imgMove(){
		$("li:first").appendTo("ul")
	}

</script>
</head>
<body>
<ul>
	<li id="i"><img src="../img/01.png"/></li>
</ul>
</body>
</html>
```



### Object 2

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<style>
	div{border: 3px solid orange; margin: 5px;}
</style>
<script>
	$(function(){
		// 객체 감싸기
		// wrap() : 선택자를 특정태그로 각각 감싼다.
		$(".c1").wrap("<h1/>");
		
		// wrapAll() : 선택자를 한번에 감싼다. 
		$(".c2").wrapAll("<div/>");
		
		// wrapInner() : 선택자의 안쪽을 특정태그로 감싼다.
		$(".c2").wrapInner("<b/>");
		
		// unwrap() : 선택자의 부모를 지운다.
		$('.c2').unwrap();
		
		// each() : 여러개의 객체를 순차적으로 적용 (반복처리)
		//			  idx=0,1,2,3,4,5,6	 obj=li, li, li, li, li, li, li...
		$('#list>li').each(function(idx, obj){
			$(obj).html("<li>each() 함수를 이용한 반복실행...("+idx+")</li>");
		});
		
		// map() : 배열을 이용한 반복실행
		var arr=['Running','Walking','Marathon','Cycling','Skiing','Tracking'];
		var tag = "<select>";
		
		// 배열명.map(function(data, index){});
		arr.map(function(data, idx){
			tag += "<option>" + data + "("+idx+")</option>";
		});
		
		tag += "</select>";
		//$("body").prepend(tag);
		$("h1:first").before(tag);
		
	});
</script>
</head>
<body>
<div class="c1">객체조작 메소드 1</div>
<div class="c1">객체조작 메소드 2</div>
<div class="c2">객체조작 메소드 3</div>
<div class="c2">객체조작 메소드 4</div>

<ul id="list">
	<li>객체 조작 메소드 AAAA</li>
	<li>객체 조작 메소드 BBBB</li>
	<li>객체 조작 메소드 CCCC</li>
	<li>객체 조작 메소드 DDDD</li>
	<li>객체 조작 메소드 EEEE</li>
	<li>객체 조작 메소드 FFFF</li>
	<li>객체 조작 메소드 GGGG</li>
</ul>
</body>
</html>
```

