# JavaScript - jQuery: Event

> Fri Jul 15, 2022

---

[toc]

### Event 처리방법

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script>
	$(function(){
		// jquery 이벤트 종류
		// onclick, onmouseover, onmouseout, onmouseenter, onmouseleave
		// onkeyup, onkeydown...
		
		/* 
		// **** 이벤트 처리방법 1 --------------
		// h1 에 onclick 이 발생하면
		$("h1").click(function(){
			// 이벤트 발생시 실행되는 코드
			$("body").prepend("h1 태그를 클릭하였습니다.");
			
			// this : 이벤트가 발생한 객체
			$(this).css("background", "lightblue");
			$(this).text( $(this).text() + "A");
		}); 
		*/
		
		/* 
		// **** 이벤트 처리방법 2 --------------
		// on()		이벤트종류	  실행할함수 //
		$("h1").on('click', function(){
			$(this).css("color", "red");
		});
		*/
		
		/* 
		// **** 이벤트 처리방법 3 --------------
		// h1에 마우스 오버를 하면 글자색을 blue로 마우스 아웃을 하면 글자색을 green으로 변경
		$("h1").on({mouseenter:function(){ // 마우스 오버일때
			$(this).css('color', 'blue');
		}, mouseleave:function(){ // 마우스 아웃일때
			$(this).css('color', 'green');
		}});
		*/
		 
		/*  
		// 이벤트 처리방법 4 --------------
		// 1개의 요소에 2개이상의 이벤트를 한번에 작성하기
		$("input").on('click focus', function(){
			$(this).css('background', 'yellow');
		});
		 */
		 
		/*  
		// 이벤트 처리방법 5 --------------
		//			  이벤트종류, 이벤트대상, 실행함수
		$(document).on('click','h1',function(){
			$(this).css('background', 'cyan');
		});
		*/
		
		// 이벤트 처리방법 6 --------------
		document.getElementById("i").addEventListener('click', function(){
			$(this).css('color', 'orange');
		});
	});
</script>
</head>
<body>
<h1>jquery의 이벤트 처리</h1>
<h1 id="i">jquery의 이벤트 처리</h1>
<h1>jquery의 이벤트 처리</h1>
<h1>jquery의 이벤트 처리</h1>
<input type="button" value="123456"/>
<input type="button" value="가나다라마바"/>
<input type="button" value="ABCDEF"/>

</body>
</html>
```

