

# JavaScript - jQuery: 드랍다운 메뉴 만들기

> Mon Jul 18, 2022

---

마우스를 메뉴에 가져가면 서브메뉴가 나오는 페이지를 만들어 보겠습니다.

![Screen Shot 2022-07-18 at 9.16.35 AM](/Users/sanghyeop/Desktop/Screen Shot 2022-07-18 at 9.16.35 AM.png)



```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<style>
	body, ul, li{margin:0; padding:0; list-style-type:none;}
	.logo a:link, .logo a:visited{text-decoration:none; color:#000;}
	.logo a:hover{text-decoration:none; color:#f00;}
	
	.popMenu a:link, a:visited{text-decoration:none; color:#333;}
	.popMenu a:hover{text-decoration:none; color:#333; font-weight:bold;}
	
	.container{
		width:1000px; margin:0 auto;
	}
	/*	로고	 */
	.logo{
		height:150px; line-height:150px; text-align:center; font-size:3em;
	}
	/* 메인메뉴 */
	.popMenu{
		height:50px;
		border-top:2px solid #333;
		border-bottom:1px solid #333;
	}
	
	.popMenu>li{
		float:left; width:20%; height:50px; line-height:50px; text-align:center;
	}
	
	.popMenu>li>ul{
		background-color:white; position:relative; display:none;
	}
	
	 .section>img{width:100%;}
</style>
<script>
	$(function(){
		// 팝업메뉴
		$(".popMenu>li").hover(function(){
			// 메인메뉴가 마우스 오버일때
			$(this).children('ul').css('display', 'block');
		}, function(){
			// 메인메뉴가 마우스 아웃일때
			$(this).children('ul').css('display', 'none');
		});
	});
</script>
</head>
<body>
<div class="container">
	<!--  로고  -->
	<div class="logo"><a href="#">GIRLSDAILY</a></div>
	<ul class="popMenu">
		<li><a href="#">OUTER</a>
			<ul>
				<li><a href="#">가디건</a></li>
				<li><a href="#">자켓 & 점퍼</a></li>
				<li><a href="#">베스트</a></li>
				<li><a href="#">코트</a></li>
				<li><a href="#">세일</a></li>
			</ul>
		</li>
		<li><a href="#">TOP</a>
			<ul>
				<li><a href="#">Tee</a></li>
				<li><a href="#">Cami</a></li>
				<li><a href="#">Knit</a></li>
				<li><a href="#">Shirt & Blouse</a></li>
			</ul>
		</li>
		<li><a href="#">DRESS</a>
			<ul>
				<li><a href="#">All</a></li>
				<li><a href="#">바캉스룩</a></li>
				<li><a href="#">Set</a></li>
			</ul>
		</li>
		<li><a href="">BOTTOM</a>
			<ul>
				<li><a href='#'>레깅스</a></li>
				<li><a href='#'>힐링두엘브</a></li>
				<li><a href='#'>히든벤딩시리즈</a></li>
				<li><a href='#'>스커트</a></li>
				<li><a href='#'>팬츠</a></li>
			</ul>
		</li>
		<li><a href="">SUMMER</a>
			<ul>
				<li><a href='#'>HOLI스윕웨어</a></li>
				<li><a href='#'>At 22°C</a></li>
			</ul>
		</li>
	</ul>
	<div class="section">
		<img src="../img/background01.jpeg"/>
	</div>
</div>

</body>
</html>
```

