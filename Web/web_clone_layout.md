# Web: 레이아웃 만들기

> Mon Jul 11, 2022

---

[toc]

아래의 웹페이지의 레이아웃을 만들어보겠습니다.

![tab3](web_clone_layout.assets/tab3.png)

index.html, layoutStyle.css 로 나누어서 만들어 보겠습니다.



### index.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<link rel="stylesheet" href="layoutStyle.css" type="text/css"/>
</head>
<body>
<h1>레이아웃 만들기</h1>
<div id="div">
	<!-- Q&A -->
	<ul id="qna">
		<li>
			<div>Q. <span class="fnt_bold">할리우드 영화에서...</span></div>
			<div>A. 냉전시대의 산물이다...</div>
		</li>
		<li>
			<div>Q. <span class="fnt_bold">독도 이름의 유래를 알려주세요.</span></div>
			<div>A. 독도와 관련된 명칭은 시대에 따라 문헌에 아주 다양하게 나옵니다...</div>
		</li>
		<li>
			<div>Q. <span class="fnt_bold">매실청은 당뇨환자에게 해로운건가요?</span></div>
			<div>A. 매실청은 설탕으로 매실을 발효시킨 식품으로 당분이 많이...</div>
		</li>
		<li>
			<div>Q. <span class="fnt_bold">108배가 다이어트에 도움이 되나요?</span></div>
			<div>A. 108배를 하면 땀도 굉장히 많이 나고 전신운동이 되기 때문에 운동 효과...</div>
		</li>
	</ul>
	<!-- Topic -->
	<div id="topic">
		<img src="tab2_1.png"/>
	</div>
	<!-- Bottom -->
	<div id="btmDiv">
		<ul>
			<li>
				<img src="tab2_2.png"/><br/>
				<span class="fnt_bold">견과류의 하루 적정 섭취량</span><br/>
				견과류
			</li>
			<li>
				<img src="tab2_3.png"/><br/>
				<span class="fnt_bold">날벌레가 빛에 몰려드는 이유</span><br/>
				곤충
			</li>
			<li>
				<img src="tab2_4.png"/><br/>
				<span class="fnt_bold">냉면의 유래와 역사</span><br/>
				음식
			</li>
		</ul>
	</div>
</div>
</body>
</html>
```





### layoutStyle.css

```css
@charset "UTF-8";
ul, li{
	margin:0;
	padding:0;
	list-style-type:none;
}
body{
	font-size:13px;
}
.fnt_bold{
	font-weight:bold;
}
#div{
	width:579px;
	padding:10px;
	border: 1px solid #ddd;
	margin: 0px auto;
}
#qna{
	width: 400px;
	float: left;
}
#qna>li{
	width:145px;
	height:131px;
	border:1px solid #ddd;
	float:left;
	margin:0 20px 20px 0;
	padding:10px 10px 10px 20px;
}
#qna>li>div:first-child{
	height:50px;
}
#qna>li>div{
	text-indent:-1em;
}
#topic{
	margin-bottom:22px;
}
#btmDiv{
	
}
#btmDiv>ul{
	overflow:auto;
}
#btmDiv>ul>li{
	float:left;
	margin:0 20px 0px 0px;
	width:179px;
}
#btmDiv>ul>li:last-child{
	margin-right:0px;
}
```

