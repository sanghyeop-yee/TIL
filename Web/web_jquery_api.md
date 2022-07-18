# JavaScript - jQuery: jQuery API

> Mon Jul 18, 2022

---

[toc]

jQuery Library 를 활용하는 방법을 알아봅시다.

프로젝트내에서 사용하기 위해 폴더 정리가 필수입니다.

webApp > src > main > webapp 안에 js_css 라는 폴더를 만들어 다운받은 API 를 관리합니다. 

사용시에는 jquery 를 불러오는 스크립트 위에 link 스크립트로 연결을 해줍니다.



### jQuery UI : Easing HTML

`<link rel="stylesheet" href="/webApp/js_css/jquery-ui.min.css" type="text/css"/>`

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- 
		easing 종류 확인가능
		http://jqueryui.com/easing/ -> API Documentation
		
		jqueryui 가 필요함
		https://jqueryui.com/download/all 에서 jquery-ui-1.13.2.zip 을 다운로드 후 압축푼다.
		
		1. jquery-ui.min.css 스타일시트 연결
		2. jquery.min.js
		3. jquery-ui.min.js 
		
-->
<link rel="stylesheet" href="/webApp/js_css/jquery-ui.min.css" type="text/css"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="/webApp/js_css/jquery-ui.min.js"></script>

<style>
	img{position:relative};
</style>

<script>
	$(function(){
		// easing: 움직이는 모양								  easing 종류
		$("img").first().animate({marginLeft: "1200px"}, 6000, "easeOutBounce");
		$("img:eq(1)").animate({marginLeft: "1000px"}, 5000, "easeOutBack");
		
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





### CKEDITOR

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="//cdn.ckeditor.com/4.19.0/full/ckeditor.js"></script>
<script>
	$(function(){
		// textarea 에 ckeditor 셋팅하기
		CKEDITOR.replace('content');
		
		$("#boardFrm").submit(function(){
			// 제목이 입력여부
			if($("#subject").val()==""){ // if(document.getElementById("subject").value == "") => 자바스크립트로도 작성가능
				alert("제목을 입력하세요.");
				return false;
			}
			
			// 글내용 입력여부
			// ckeditor 로 셋팅 textarea 의 값을 가져오는 함수
			if(CKEDITOR.instances.content.getData() == ""){
				alert("글내용을 입력하세요." + CKEDITOR.instances.content.getData());
				return false;
			}
			console.log(CKEDITOR.instances.content.getData());
			return false;
		});
	});
</script>
</head>

<body>
<h1>편집기</h1>
<form method="post" id="boardFrm" action="aaa.jsp">
	<ul>
		<li>제목</li>
		<li><input type="text" name="subject" id="subject"/></li>
		<li>글내용</li>
		<li><textarea name="content" id="content"></textarea></li>
		<li><input type="submit" value="글등록"/>
	</ul>
</form>
</body>
</html>
```



### innerfade

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- 
	innerfade
	
	http://gist.github.com/coolniikou/665255

 -->

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="/webApp/js_css/jquery.innerfade.js"></script>
<style>
	ul, li{
		margin: 0; padding: 0; list-style-type: none;
	}
	#imgInner img{width: 500px; height: 400px;}
</style>
<script>
	$(function(){
		
		//
		$("#imgInner").innerfade({
			animationtype: 'slide' // 'slide', 'fade', 기본 fade
			,speed: 'slow' // 'slow', 'fast', 'normal', 밀리초
			,timeout: 5000 // 쇼대기 시간
			,type: 'random' // 'random', 'sequence': 기본, random_start
		});
		
	});
</script>
</head>
<body>
<div>
	<ul id="imgInner">
		<li><a href="#"><img src='/webApp/img/sf01.jpeg'/></a></li>
		<li><a href="#"><img src='/webApp/img/sf02.jpeg'/></a></li>
		<li><a href="#"><img src='/webApp/img/sf03.jpeg'/></a></li>
		<li><a href="#"><img src='/webApp/img/sf04.jpeg'/></a></li>
		<li><a href="#"><img src='/webApp/img/sf05.jpeg'/></a></li>
	</ul>
</div>

</body>
</html>
```



### bxSlider

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- 
	https://github.com/stevenwanderski/bxslider-4 에서 다운로드
	
	1. jquery
	2. easing
	3. bxslider 스크립트
	4. bxslider 스타일시트
 -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="/webApp/js_css/jquery.easing.1.3.js"></script>
<script src="/webApp/js_css/jquery.bxslider.js"></script>
<link rel="stylesheet" href="/webApp/js_css/jquery.bxslider.css" type="text/css"/>
<style>
	#imgSlider img{width:800px; height:400px;}
	ul, li{margin:0; padding:0}
</style>
<script>
	$(function(){
		$("#imgSlider").bxSlider({
			mode:'horizontal' // 화면전환방식 'horizontal', 'vertical', fade
			,slideWidth: 800 // 슬라이드 폭
			,slideHeight: 450 // 슬라이드 높이
			,speed: 5000
			,auto: true // 자동시작 true, false(기본)
			,randomStart: true // 랜덤 시작 true, false(기본)
			,captions: true // 타이틀 속성의 값을 설명으로 표시 true, false(기본)
			,infiniteLoop: false // 반복여부
			,hideControlOnEnd: false // 좌우이동 컨트롤 보여주기 true: 처음과 마지막에 컨트롤 표시안함
			
			,useCSS : false // easing 사용 여부 true(사용안함), false(사용함)
			,easing: 'easeOutElastic'
			/* 
			,onSliderLoad: function(){ // 슬라이더가 로딩이 완료되면 호출되는 콜백함수
				alert("onSliderLoad 콜백함수 실행됨");
			}
		 	*/
		 	,onSlideAfter: function(){ // 슬라이더가 움직인 후 호출되는 콜백함수
		 		console.log("onSlideAfter 실행됨.");
		 	}
		});
	});
</script>


</head>
<body>
<div>
	<ul id="imgSlider">
		<li><a href="https://www.nate.com"><img src='/webApp/img/sf01.jpeg' title="네이트로 이동하기"/></a></li>
		<li><a href="https://www.naver.com"><img src='/webApp/img/sf02.jpeg' title="네이버로 이동하기"/></a></li>
		<li><a href="#"><img src='/webApp/img/sf03.jpeg' title="세번째 이미지 설명.."/></a></li>
		<li><a href="#"><img src='/webApp/img/sf04.jpeg' title="네번째 이미지 설명.."/></a></li>
		<li><a href="#"><img src='/webApp/img/sf05.jpeg' title="다섯번째 이미지 설명.."/></a></li>
	</ul>
</div>

</body>
</html>
```





### accordionImageMenu & jquery-ui.min.js

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- 
	https://github.com/alaingoga 에서 다운로드
	
	1. accordionImageMenu.css
	2. jquery
	3. jqueryui
	4. jquery.accordionImageMenu.js
 -->
<link rel="stylesheet" href="/webApp/js_css/accordionImageMenu.css" type="text/css"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="/webApp/js_css/jquery-ui.min.js"></script>
<script src="/webApp/js_css/jquery.accordionImageMenu.js"></script>
<script>
	$(function(){
		$("#acc").AccordionImageMenu({
			position: 'horizontal' // 'horizontal', 'vertical'
			,openItem: 3 // 기본선택목록, index 위치의 내용이 크게나옴
			,openDim: 500 // 확장된 목록의 폭
			//,closeDim: 100
			
			// width, height
			//,width: 1400
			,height: 500
			,duration: 3000 //전환속도
			,effect: 'easeOutBounce'
			,border: 1
			,color:'yellow' // 배경색
		});
	});
</script>
</head>
<body>
<div>
	<ul id="acc">
		<li><a href="#"><img src='/webApp/img/sf01.jpeg'/></a></li>
		<li><a href="#"><img src='/webApp/img/sf02.jpeg'/></a></li>
		<li><a href="#"><img src='/webApp/img/sf03.jpeg'/></a></li>
		<li><a href="#"><img src='/webApp/img/sf04.jpeg'/></a></li>
		<li><a href="#"><img src='/webApp/img/sf05.jpeg'/></a></li>
	</ul>
</div>
</body>
</html>
```



### dialog datepicker

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- 
	1. jquery-ui css
	2. jquery
	3. jquery-ui js
 -->
 <link rel="stylesheet" href="/webApp/js_css/jquery-ui.min.css" type="text/css"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="/webApp/js_css/jquery-ui.min.js"></script>
<style>
	#dialogOpen, #schedule{font-size:2em; background:#ffd; text-align:center;}
	#schedule{background: #dff;}
	
	h2{text-align: center;}
	input[type=text], label{margin-bottom:13px; padding:5px; width:95%;}
</style>
<script>
	$(function(){
		// 일정등록 클릭시
		$("#dialogOpen").click(function(){
			$("#dialog").dialog('open');// 일정등록을 클릭하면 다이얼로그 창 보이게 하기
		});
		// 다이얼로그
		$("#dialog").dialog({
			autoOpen: false // 실행시 자동열림 설정 (true: 기본, false)
			,buttons:{
				submit : function(){
					var name = $("#event-name").val();
					var date = $("#event-date").val();
					
					$("#schedule").append("<p>"+name+" : "+date+"</p>"); // schedule구역에 마지막에 붙이기
					
					$("#event-name").val(''); // event-name 의 값을 문맥으로 바꾸기
					$("#event-date").val('');
				},
				reset : function(){
					$("#event-name").val(''); 
					$("#event-date").val('');
				},
				close : function(){
					$("#event-name").val(''); 
					$("#event-date").val('');
					$("#dialog").dialog("close");
				}
			}
			,modal: true // 창이 떴을때 다른 화면 사용불가 
			
		});
		
		// 날짜입력박스 : datepicker 셋팅 
		$("#event-date").datepicker({
			dateFormat : 'yy년 mm월 dd일' // y: 년도 2자리, yy: 년도 4자리, yy-mm-dd
			,numberOfMonths : 3
		});
	});
</script>
</head>
<body>
<div id="dialogOpen">일정등록</div>
<hr/>
<div id="schedule">일정이 표시되는 곳</div>

<!-- 다이얼로그 창 -->
<div id="dialog" title="JQUERY UI Dialog">
	<h2>일정을 등록하세요.</h2>
	<label for="event-name">일정이름</label> : 
	<input type="text" id="event-name"/>
	<label for="event-date">날짜선택</label> :
	<input type="text" id="event-date"/>
	
</div>
</body>
</html>
```

