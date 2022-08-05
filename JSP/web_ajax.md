# Ajax

> Tue Jul 19, 2022

---

[toc]

### Ajax 란?

Ajax는 JavaScript의 라이브러리중 하나이며 Asynchronous Javascript And Xml(비동기식 자바스크립트와 xml)의 약자입니다. 브라우저가 가지고있는 XMLHttpRequest 객체를 이용해서 전체 페이지를 새로 고치지 않고도 페이지의 일부만을 위한 데이터를 로드하는 기법 이며 Ajax를 한마디로 정의하자면 JavaScript를 사용한 비동기 통신, 클라이언트와 서버간에 XML 데이터를 주고받는 기술이라고 할 수 있겠습니다.

일반적인 ajax 방법은 다음과 같습니다.

```javascript
$.ajax({
	url: '', // Controller의 mapping값
	type: '',  // get, post 방식 中
	data: '',  // Controller로 보낼 데이터
	contentType: '',  // 보내는 data의 타입
	dataType: '', // 받을 데이터 타입
	success: function() {},  // 정상적으로 return 받았을 때 실행할 함수
	error: function(){} // 실패했을 때 작동할 함수
});
```



ajax 를 통해서 text, json, xml 문서를 가져오는 방법을 배워봅시다.



### TEXT 가져오기

#### abcd.txt

```javascript
<img src='/webApp/img/sf01.jpeg'/><br/>
<h2>AJAX란,</h2> JavaScript의 라이브러리중 하나이며 <i>Asynchronous Javascript And Xml(비동기식 자바스크립트와 xml)</i>의 약자이다. 
브라우저가 가지고있는 <b>XMLHttpRequest</b> 객체를 이용해서 전체 페이지를 새로 고치지 않고도 페이지의 일부만을 위한 데이터를 
로드하는 기법 이며 JavaScript를 사용한 비동기 통신, 클라이언트와 서버간에 XML 데이터를 주고받는 기술이다.
즉, 쉽게 말하자면 자바스크립트를 통해서 서버에 데이터를 요청하는 것이다.
```



#### XMLHttpRequest.html (자바스크립트 활용)

>  XMLHttpRequest : Ajax 처리를 하는 자바스크립트 내장객체 입니다.

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



#### jq_ajax01_test.html (jQuery 활용)

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script>
	$(function(){
		$("#result>button").click(function(){
			// 서버에서 abcd.txt 파일의 내용을 가져오기
			//				 파일명
			$("#result").load("abcd.txt");
		});
	});
</script>
</head>
<body>
<h1>jquery에서 ajax처리(text)</h1>
<div id="result" style="background:#ddd;"></div>
	<button>클릭하시면 비동기식으로 text파일의 내용을 가져옴.</button>
</body>
</html>
```





### JSON 가져오기

#### ajax_data.json

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



#### ajax_json.html

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





### XML 가져오기 

> XML (Extensible Markup Language) : 데이터를 저장하고 전달할 목적으로 만들어졌으며, 저장되는 데이터의 구조를 기술하기 위한 언어입니다



#### ajax_data.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jusorok>
	<row>
		<username>John</username>
		<tel>650-404-4005</tel>
		<addr>520 21st</addr>
	</row>
	<row>
		<username>MJ</username>
		<tel>600-500-0000</tel>
		<addr>500 21st</addr>
	</row>
	<row>
		<username>Jono</username>
		<tel>600-500-1111</tel>
		<addr>500 22st</addr>
	</row>
	<row>
		<username>Andrew</username>
		<tel>600-500-2222</tel>
		<addr>500 23st</addr>
	</row>
	<row>
		<username>Kaycee</username>
		<tel>600-500-4444</tel>
		<addr>500 25st</addr>
	</row>
</jusorok>
```



#### RSS 이란?

그 사이트를 직접 방문하지 않고, 새 기사들만 자신의 컴퓨터로 "배달"된다면 편리하겠죠? 새 기사들의 제목만, 또는 새 기사들 전체를 뽑아서 하나의 파일로 만들어 놓은 것입니다. 새로운 읽을거리가 자주 올라오는 "뉴스형", "블로그형" 사이트에서 주로 제공됩니다. 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss xmlns:atom="http://www.w3.org/2005/Atom"
	xmlns:content="http://purl.org/rss/1.0/modules/content/"
	xmlns:dc="http://purl.org/dc/elements/1.1/"
	xmlns:sy="http://purl.org/rss/1.0/modules/syndication/"
	xmlns:media="http://search.yahoo.com/mrss/" version="2.0">
	<channel>
		<title>
<![CDATA[ 조선일보 ]]>
		</title>
		<link>https://www.chosun.com</link>
		<atom:link
			href="https://www.chosun.com/arc/outboundfeeds/rss/" rel="self"
			type="application/rss+xml" />
		<description>
<![CDATA[ 1등 인터넷뉴스 조선닷컴 | 전체기사 ]]>
		</description>
		<lastBuildDate>Tue, 19 Jul 2022 04:28:19 +0000</lastBuildDate>
		<language>ko</language>
		<category>news</category>
		<ttl>1</ttl>
		<sy:updatePeriod>hourly</sy:updatePeriod>
		<sy:updateFrequency>1</sy:updateFrequency>
		<image>
			<url>https://www.chosun.com/resizer/tMofBBp8RA5HaB4tbxnftjdS9qA=/image.chosun.com/cs_logo.png</url>
			<title>조선일보</title>
			<link>https://www.chosun.com</link>
		</image>
		<item>
			<title>
<![CDATA[ 與 “이재명도 시험 없이 비서관 채용, 사적채용 공세는 내로남불” ]]>
			</title>
			<link>https://www.chosun.com/politics/politics_general/2022/07/19/VQXZLMP6WZCIJG7QOX67J7NUM4/</link>
			<guid isPermaLink="true">https://www.chosun.com/politics/politics_general/2022/07/19/VQXZLMP6WZCIJG7QOX67J7NUM4/</guid>
			<dc:creator>
<![CDATA[ 김명진 기자 ]]>
			</dc:creator>
			<description />
			<pubDate>Tue, 19 Jul 2022 03:42:00 +0000</pubDate>
			<content:encoded>
<![CDATA[ <p>국민의힘 의원들이 19일 대통령실 사적 채용 논란과 관련해 야당에서 연일 공세를 벌이는 것을 두고 반박에 나섰다. 더불어민주당 이재명 의원이 경기도지사 시절 기용한 별정직 공무원 채용 사례, 문재인 정권의 청와대 직원 채용 사례 등을 거론하며 “그럴 듯한 프레임을 씌운 내로남불 공세”라고 맞받았다.</p><img src="https://www.chosun.com/resizer/0VInrfiB2idhziyoINZCuhBB-H8=/cloudfront-ap-northeast-1.images.arcpublishing.com/chosun/F3FSXLMU22T5WMFYXXKPDQYEVY.jpg" alt="김기현 국민의힘 의원이 12일 서울 여의도 국회 의원회관에서 열린 위기를 넘어 미래로, 민·당·정 토론회에서 인사말을 하고 있다. /뉴스1" height="2970" width="4098"/> ]]>
			</content:encoded>
		</item>
		<item>
			<title>
<![CDATA[ "커브 좋다. 기대 된다" SSG 새 외국인 투수 베일 벗었다 ]]>
			</title>
			<link>https://www.chosun.com/sports/sports_photo/2022/07/19/O7OGHV4SBYORKGO773TOTNBLBA/</link>
			<guid isPermaLink="true">https://www.chosun.com/sports/sports_photo/2022/07/19/O7OGHV4SBYORKGO773TOTNBLBA/</guid>
			<dc:creator>
<![CDATA[ 스포츠조선 = 나유리 기자 ]]>
			</dc:creator>
			<description>
<![CDATA[ [스포츠조선 나유리 기자]SSG 랜더스의 새 외국인 투수 숀 모리만도가 베일을 벗었다. 모리만도는 19일 인천 SSG랜더스필드에서 열린 파주 챌린저스와의 연습 경기에 선발 등판했다. 모리만도는 SSG가 이반 노바를 퇴출한 후 영입한 대체 외국인 투수다. 노바가 부상과 부진 등의 이유로 3승4패 평균자책점 6.50의 성적을 기록한 이후 팀을 떠났고, 대체 선 ]]>
			</description>
			<pubDate>Tue, 19 Jul 2022 04:26:38 +0000</pubDate>
			<content:encoded>
<![CDATA[ <img src="https://www.chosun.com/resizer/XWM4wgrBPVQmDmmECbmPuHc2Zs8=/cloudfront-ap-northeast-1.images.arcpublishing.com/chosun/VROGAQ6GHPTV5A6MPWERDMH5YI.jpg" alt="SSG와 계약한 숀 모리만도. 사진제공=SSG 랜더스" height="683" width="640"/><p>[스포츠조선 나유리 기자]SSG 랜더스의 새 외국인 투수 숀 모리만도가 베일을 벗었다.</p> ]]>
			</content:encoded>
			<media:content
				url="https://www.chosun.com/resizer/XWM4wgrBPVQmDmmECbmPuHc2Zs8=/cloudfront-ap-northeast-1.images.arcpublishing.com/chosun/VROGAQ6GHPTV5A6MPWERDMH5YI.jpg"
				type="image/jpeg" height="683" width="640">
				<media:description type="plain">
<![CDATA[ SSG와 계약한 숀 모리만도. 사진제공=SSG 랜더스 ]]>
				</media:description>
			</media:content>
		</item>
```



#### jq_ajax03_xml.html

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

