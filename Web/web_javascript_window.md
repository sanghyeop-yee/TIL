# JavaScript: Window 내장객체

> Mon Jul 11, 2022

---

[toc]

popup.html 을 따로 만들어서 띄울 팝업창 내용을 정의합니다.

팝업창 띄우기라는 이벤트를 버튼을 클릭해서 실행하면 openPopup() 메소드가 실행되고 `window.open()` 으로 팝업창을 띄웁니다.

		1. 새창에 띄울 파일명

		2. 창이름: 창이름이 없으면 새창에 반복적으로 만들어짐

		3. 옵션: width, height, left, top

### popup.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style>
	img{width:100%;}
</style>
</head>
<body>
	<img src="../img/02.png"/>
</body>
</html>
```



### window.html

같은 팝업창을 열고 닫기 위해서 `var win` 로 변수를 하나 선언해줍니다.

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
	var win;
	function openPopup(){
		// 팝업창 띄우기
		// 1. 새창에 띄울 파일명
		// 2. 창이름: 창이름이 없으면 새창에 반복적으로 만들어짐
		// 3. 옵션: width, height, left, top
		win = window.open("popup.html","nft","width=400px, height=350px, left=200px, top=200px");
		
		var x = screen.width/2 - win.outerWidth/2
		var y = screen.height/2 - win.outerHeight/2
		
		win.moveTo(x, y);
	}
	function closePopup(){
		win.close();
	}
	function windowMove(){
		// 창이동하기
		// 절대 좌표
		//win.moveTo(500,300);
		
		// 상대 좌표
		win.moveBy(-10, -10);
	}
	function windowSize(){
		// 창의 크기를 변경
		// 절대 크기 w, h
		//win.resizeTo(800, 600);
		// 상대 크기
		win.resizeBy(10, 10);
	}
	function winInfor(){
		var txt="";
		
		// 윈도우의 좌표구하기
		txt += "x좌표->"+window.screenX+"<br/>";
		txt += "y좌표->"+window.scrrenY+"<br/>";
		
		// 테두리와 제목을 포함한 폭과 높이
		txt += "outerWidth->"+ window.outerWidth+ "<br/>";
		txt += "outerHeight->"+ window.outerHeight+ "<br/>";
		
		// 테두리와 제목을 제외한 폭과 높이
		txt += "innerWidth->"+ window.innerWidth+"<br/>";
		txt += "innerHeight->"+ window.innerHeight+"<br/>";
		
		// screen 의 폭과 높이
		txt += "screen.width->"+ screen.width+"<br/>";
		txt += "screen.height->"+ screen.height+"<br/>";
		
		document.querySelector("#view").innerHTML = txt;
	}

/* 	
	function popupMiddle(){
		var popupWidth=400;
		var popupHeight=300;
		var popupX=(window.screen.width/2)-(popupWidth/2);
		var popupY=(window.screen.height/2)-(popupHeight/2);
		win = window.open('popup.html', 'nft', 'status=no, height=' + popupHeight  + ', width=' + popupWidth  + ', left='+ popupX + ', top='+ popupY);
	} 
*/
	
</script>
</head>
<body onload="openPopup(); winInfor();">
<h1>window 내장객체</h1>
<input type="button" onclick="openPopup()" value="팝업창띄우기"/>
<input type="button" onclick="closePopup()" value="팝업창닫기"/>
<input type="button" onclick="windowMove()" value="팝업창이동"/>
<input type="button" onclick="windowSize()" value="창크기변경"/>
<!-- <input type="button" onclick="popupMiddle()" value="창가운데"/> -->
<div id="view" style="border:1px solid gray;"></div>
</body>
</html>
```

