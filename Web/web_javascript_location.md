# JavaScript: Location 객체

> Tue Jul 12, 2022

---

[toc]

#### Location 객체

location 객체는 현재 브라우저에 표시된 HTML 문서의 주소를 얻거나, 브라우저에 새 문서를 불러올 때 사용할 수 있습니다.

 

이 객체는 Window 객체의 location 프로퍼티와 Document 객체의 location 프로퍼티에 같이 연결되어 있습니다.

location 객체의 프로퍼티와 메소드를 이용하면, 현재 문서의 URL 주소를 다양하게 해석하여 처리할 수 있습니다.



```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>location 내장객체</h1>
<script>
	document.write("protocol=>"+ location.protocol);
	document.write("<br/>hostname=>"+ location.hostname);
	document.write("<br/>port=>" + location.port);
	
	document.write("<br/>host=>"+ location.host); // hostname+port
	
	// location.reload(); // 현재페이지를 재실행
	
	// 페이지 이동
	// location.href = "https://www.naver.com";
	location.href = "../index.html";
</script>
</body>
</html>
```

