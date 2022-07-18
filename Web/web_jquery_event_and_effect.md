# JavaScript - jQuery: Event & Effect

> Mon Jul 18, 2022

---

[toc]

### Event Key

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script>
	$(function(){
		// keyup, keydown, keypress, keylease
		$ ("#comment").keyup(function(){
			var cnt = 100 - $("#comment").val().length;
			$('#cnt').text(cnt+'');
			
			if(cnt<=0){
				//$("#comment").prop("disabled", true);
				//$(this).off();
			}
		
		});
	});
</script>
</head>
<body>
<div> 남은문자수 : <span id='cnt'>100</span></div>
<textarea id = "comment" rows = "5" cols = "50" maxlength = "100"></textarea>
</body>
</html>
```



### Event Trigger

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<style>
	img {width:100px; height:100px;}
</style>
<script>
	$(function(){
		var tag = "<img src='../img/sf02.jpeg'/>";
		
		// 이벤트 발생시 실행
		$("img:first").click(function(){
			$('body').append(tag);
		});
		
		// trigger() : 이벤트를 발생시킴
		// setInterval(), setTimeout()
		
		setInterval(function(){
			// 강제로 이벤트 발생시킴
			$("img").first().trigger('click');
		}, 1000);
	});
	
</script>
</head>
<body>
<img src="../img/sf01.jpeg"/>

</body>
</html>
```



### Effect

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<style>
	img{width: 500px; height: 500px;}
</style>
<script>
	$(function(){
		// hide : 폭, 높이, 점점흐리게 숨기기
		$("#hide").click(function(){
			
			// 				사라지는 시간 : 'slow', 'fast', 'normal', 밀리초
			$("img").first().hide(4000, function(){ // 콜백함수
				//alert("종료됨");
			}); 
			
		});
		// show : 보여주기
		$("#show").on('click', function(){
			$("img:first").show(4000);
		});
		// toggle : 보여주기, 숨기기를 반복실행
		$(document).on('click', '#toggle', function(){
			$("img").first().toggle(4000);
		});
		// 점점흐리게
		$("#fadeOut").click(function(){
			$("img").first().fadeOut(4000);
		});
		// 점점진하게
		$("#fadeIn").click(function(){
			$("img").first().fadeIn(4000);
		});
		// 흐리게 진하게 반복실행
		$("#fadeToggle").click(function(){
			$("img:first").fadeToggle(3000);
		});
		$("#slideUp").click(function(){
			$("img").first().slideUp(3000);
		});
		$("#slideDown").click(function(){
			$("img").first().slideDown(3000);
		});
		$("#slideToggle").click(function(){
			$("img").first().slideToggle(3000);
		});
	});
</script>
</head>
<body>
<button id="hide">숨기기(hide)</button>
<button id="show">보여주기(show)</button>
<button id="toggle">숨기기, 보여주기(toggle)</button>
<button id="fadeOut">점점흐리게(fadeOut)</button>
<button id="fadeIn">점점보이게(fadeIn)</button>
<button id="fadeToggle">흐리게, 진하게 반복실행(fadeToggle)</button>
<button id="slideUp">slideUp</button>
<button id="slideDown">slideDown</button>
<button id="slideToggle">slideToggle</button>
<hr/>
<img src="../img/sf03.jpeg"/><br/>
<img src="../img/sf04.jpeg"/>
</body>
</html>
```





### Animate

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<style>
	div{float: left; width: 126px; height: 164px;}
	img{position:absolute;}
</style>
<script>
	$(function(){
		// animate() : 객체를 이동시킨다. 'slow', 'fast', 'normal', 밀리초
		// animate({JSON 형식으로 속성설정}, 속도)
		//			marginLeft, width, marginTop, opacity, marginRight
		$("img:first").animate({marginLeft: "800px", marginTop: "500px", opacity:0.3}, 5000)
					  delay(3000).animate({marginTop: "10px", opacity:1}, 3000);
		$("img:eq(2)").animate({marginLeft:"700px", marginTop:"400px"}, 5000);
		
		// delay() : 출발을 지연시킨다.
		$("img:eq(1)").delay(2000).animate({marginLeft:"900px", marginTop:"300px"}, 5000);
		
		// animate 중지시키기
		$('img').mouseover(function(){
			$(this).stop(); // $(this).stop(true);
		});
		$('img').mouseout(function(){
			$(this).stop(false);
		});
		
	});
</script>
</head>
<body>
<div><img src='../javascript/movie_post/m01.png'/></div>
<div><img src='../javascript/movie_post/m02.png'/></div>
<div><img src='../javascript/movie_post/m03.png'/></div>
<div><img src='../javascript/movie_post/m04.png'/></div>
<div><img src='../javascript/movie_post/m05.png'/></div>
</body>
</html>
```

