# Ajax

> Tue Jul 19, 2022

---

[toc]

### Ajax 란?

Ajax는 JavaScript의 라이브러리중 하나이며 Asynchronous Javascript And Xml(비동기식 자바스크립트와 xml)의 약자입니다. 브라우저가 가지고있는 XMLHttpRequest 객체를 이용해서 전체 페이지를 새로 고치지 않고도 페이지의 일부만을 위한 데이터를 로드하는 기법 이며 Ajax를 한마디로 정의하자면 JavaScript를 사용한 비동기 통신, 클라이언트와 서버간에 XML 데이터를 주고받는 기술이라고 할 수 있겠습니다.



ajax 를 통해서 text, json, xml 문서를 가져오는 방법을 배워봅시다.



### XMLHttpRequest

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script>
	// ajax 란 비동기식으로 웹서버와 통신을 할 수 있는 자바스크립트 라이브러리이다.
	// ajax 를 사용하면 비동기식으로 웹서버에 데이터를 보내거나 받을수 있다.
	function loadDom(){
		// 비동기식으로 서버에 접속하여 abcd.txt 파일의 정보를 전송받아 ajaxResult 에 내용을 변경한다.
		// XMLHttpRequest : ajax 처리를 하는 자바스크립트 내장 객체
		
		// 1. 객체 생성
		var xHttp = new XMLHttpRequest(); // 웹서버와 통신하는 객체를 생성한다.
		
		// 2. 서버에서 데이터 넘어오면 전송받을 기능 구현
		xHttp.onreadystatechange = function(){
			// 서버에서 정보가 오면 이벤트에 의해서 실행되는 함수
			// 전송결과 확인할 수 있는 변수 
			// readyState : XMLHttpRequest 의 처리결과를 가지고 있음
			//				0: 초기화 실패, 1: 서버연결, 2: 요청이 접수됨, 3. 처리요청, 4. 요청이 완료됨
			// status : 요청처리 상태 번호를 반환
			//			200: 정상처리, 403: 금지, 404: 찾을수 없음
			// statusText : 'OK', 'Not Found',
			// responseText : 실행결과 값 -> 서버에서 전송받은 내용이 있는 변수 
			console.log()
			if(this.readyState==4 && this.status==200){ // 정상처리됨
				document.getElementById("ajaxResult").innerHTML = this.responseText;
			}else{ // 전송실패
				document.getElementById("ajaxResult").innerHTML = "서버에서 정보를 가져오지 못하였습니다.";
			}
		}
		
		// 3. 객체열기
		//		   전송방식, 가져올 데이터파일명, 동기식(false) or 비동기식(true)
		xHttp.open('GET', 'abcd.txt', true);
		// 4. 객체를 보냄 : 실제 서버에 접속
		xHttp.send(); // 서버에 정보를 요청함
	}
</script>
</head>
<body>
<h1>자바스크립트를 이용한 ajax 테스트</h1>
<button onclick="loadDom()">클릭하면 서버에서 데이터를 비동기식으로 가져옴</button>
<div id="ajaxResult" style="border:1px solid gray; height:200px;"></div>
</body>
</html>
```



### ajax_data.json

```javascript
{
	"rank":[
			{"name":"John", "local":"San Francisco"},
			{"name":"MJ", "local":"Oakland"},
			{"name":"Kaycee", "local":"Seoul"},
			{"name":"Andrew", "local":"Yang Yang"},
			{"name":"Jono", "local":"LA"},
			{"name":"Erik", "local":"New York"}
			]
}
```



### ajax_json.html

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script>
	$(function(){
		$("#ajaxBtn").on('click', function(){
			// 서버에서 json데이터 가져오기
			$.ajax({
				url: 'ajax_data.json', // 서버에 접속할 대상
				dataType: 'json',
				success: function(result){ // 서버에서 정상전송을 받았을때
					console.log(result);
					$("#result").append("<ol type='1'>");
					$.each(result.rank,function(idx, data){ // 배열을 반복실행하는 jquery
						// <li>John : San Francisco </li>
						$("#result>ol").append("<li>" + data.name + " : " + data.local + "</li>");
					});
					$("#result").append("</ol>");
				},
				error: function(request, status, error){ // 서버에서 전송을 받지 못했을때
					console.log("전송실패...");
					console.log("request.code=", request.code);
					console.log("error Message=", request.responseText);
					console.log("error=", error);
				}
			});
		});
	});
</script>
</head>
<body>
<button id='ajaxBtn'>클릭하면 비동기식으로 서버에서 json 데이터 가져옴</button>
<div id="result"></div>
</body>
</html>
```





### XML

#### RSS 이란?

그 사이트를 직접 방문하지 않고, 새 기사들만 자신의 컴퓨터로 "배달"된다면 편리하겠죠? 새 기사들의 제목만, 또는 새 기사들 전체를 뽑아서 하나의 파일로 만들어 놓은 것입니다. 새로운 읽을거리가 자주 올라오는 "뉴스형", "블로그형" 사이트에서 주로 제공됩니다. 

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script>
	$(function(){
		$('#ajaxXml').click(function(){
			// xml 정보를 비동기식으로 가져오기
			$.ajax({
				url : 'ajax_data.xml',
				dataType : 'xml',
				success : function(result){
					console.log(result)
					
					if($(result).find('row').length>0){ // 레코드가 있으면 
						$(result).find('row').each(function(idx, row){
							var name = $(this).find('username').text();
							var tel = $(this).find('tel').text();
							var addr = $(this).find('addr').text();
							
							var tag = "<tr><td>"+name+"</td><td>"+tel+"</td><td>"+addr+"</td></tr>";
							$("#memList").append(tag);
						});
					}
					
				}, error:function(){
					console.log("가져오기 실패...");
				}
			})
		});
		
		// 뉴스 가져오기
		$("#newsRss").click(function(){
			$.ajax({
				url: 'newsRss.xml',
				dataType: 'xml',
				success: function(result){
					
					$(result).find('item').each(function(){
						var title = $(this).find('title').text();
						var link = $(this).find('link').text();
						var desc = $(this).find('description').text();
						var pubDate = $(this).find('pubDate').text();
						var author = "";
						if(desc!=""){
							author = desc.substring(desc.indexOf('[')+1, desc.indexOf(']'));
						}
						var date = new Date(pubDate);
						var y = date.getFullYear();
						var m = date.getMonth()+1;
						var d = date.getDate();
						
						var h = date.getHours();
						var mm = date.getMinutes();
						
						pubDate = y + '-' + m + '-' + d + ' ' + h + ':' + mm;
						
						console.log(title);
						console.log(link);
						console.log(desc);
						console.log(pubDate);
						console.log(author);
						
						var tag = "<li><b><a href='"+link+"'>"+title+"</a></b>("
						if(author!==""){
							tag += author+",";
						}
						tag += pubDate+")<br/>";
						if(desc==""){
							tag+="</li>";
						}else{
							tag += desc+"</li>";
						}
						
						$("#news>ol").append(tag);
					});
					
				}, error:function(){
					console.log("오류발생")
				}
			});
		});
	});
</script>
</head>
<body>
<div class="container">
	<button id="ajaxXml">ajax실행(xml정보가져오기)</button>
	<table id="memList" width='700' border='1'></table>
	<hr/>
	<button id="newsRss">뉴스 가져오기</button>
	<div id="news">
		<ol></ol>
	</div>
</div>
</body>
</html>
```

