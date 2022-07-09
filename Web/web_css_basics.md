# CSS Basics

> Fri Jul 8, 2022

---

[toc]

### Media 쿼리 : 반응형 스타일 지정

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<meta name="viewport" content="width-device-width. initial-scale=1"/>
<style>
	ul, li{margin:0; padding:0; list-style-type:none;}
	li{
		margin:5px 0; background:lightblue; float:left; width:100%;
	}
	/* media 쿼리를 이용하여 반응형 스타일 지정 */
	@media all and (min-width:320px) and (max-width:600px){
		li{background-color:yellow;}
	}
	@media all and (min-width:601px) and (max-width:800px){
		li{background-color:pink;}	
	}
	@media all and (min-width:801px){
		li{background-color:orange; color:#fff;}
		img{display:block;}
	}
	
</style>
</head>
<body>
<ul>
	<li>오토모빌리 람보르기니(Automobili Lamborghini)의 한국 공식 딜러 람보르기니 서울(SQDA 모터스)은 공도와 트랙 주행을 모두 만족시키는 새로운 V10 슈퍼 스포츠카 ‘우라칸 테크니카’(Huracán Tecnica) 를 출시했다. 지난 4월 우라칸 테크니카가 전 세계 최초로 공개된 이후 불과 석 달 만이다.</li>
	<li>오토모빌리 람보르기니(Automobili Lamborghini)의 한국 공식 딜러 람보르기니 서울(SQDA 모터스)은 공도와 트랙 주행을 모두 만족시키는 새로운 V10 슈퍼 스포츠카 ‘우라칸 테크니카’(Huracán Tecnica) 를 출시했다. 지난 4월 우라칸 테크니카가 전 세계 최초로 공개된 이후 불과 석 달 만이다.</li>
	<li>오토모빌리 람보르기니(Automobili Lamborghini)의 한국 공식 딜
	<li><img src="../img/02.png"></li>
</ul>

</body>
</html>
```

