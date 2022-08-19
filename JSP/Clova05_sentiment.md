# Naver Cloud Platform API : Sentiment

> Fri Aug 19, 2022

---



이번에는 텍스트 데이터에서 감정분석을 할 수 있는 API 를 활용해봅시다.



#### home.jsp



#### Clova05_sentiment_controller.java

```java
package com.cali.clova.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.ModelAndView;

@RestController
public class Clova05_sentiment_controller {
	
	@GetMapping("/clova/sentiment") // 
	public ModelAndView sentiment() {
		ModelAndView mav = new ModelAndView();
		mav.setViewName("clova/sentiment"); // view 주소
		return mav;
	}
}
```



#### sentiment.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
		<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
		<script>
			$(function(){
				$("#sentiBtn").click(function(){
					
					$.ajax({
						type: "post",
						dataType: "text",
						async: false,
						url: "/clova/sentimentOk", // mapping 주소
						data: {
							content: $("#content").val()
						},
						success: function(result){
							
						},error: function(e){
							console.log(e.responseText);
						}
					});
				});
			});
		</script>
	</head>
<body>
	<h2>Sentiment API</h2>
	<p>글에 담긴 감정을 분석하고 감정이 표현된 부분을 추출해 주는 서비스로 블로그, 댓글, SNS 등 한글로 작성된 글 속에 표현된 감정을 분석해 주는 API입니다.
	텍스트 데이터를 분석해서 해당 단어/문장/문구 내용의 감정을 분석하는 서비스로 그 결과를 반환하는 HTTP 기반의 REST API입니다.</p>
	<textarea name="content" id="content" style="width:600px; height:400px">
	싸늘하다. 가슴에 비수가 날아와 꽂힌다.
	한강이 넘치고 풍량이 부니 기분이 좋다.
	행복한 일이 많을 것 같다.
	
	서시
	죽는 날까지 하늘을 우러러
	한 점 부끄럼이 없기를
	잎새에 이는 바람에도
	나는 괴로워했다.
	별을 노래하는 마음으로
	모든 죽어가는 것을 사랑해야지
	그리고 나한테 주어진 길을
	걸어가야겠다.
	오늘 밤에도 별이 바람에 스치운다.
	윤동주
	</textarea>
	<input type="button" id="sentiBtn" value="감정평가(neutral:중립, positive:긍정, negative:부정"/>
	<div id="sentiResult"style="background-color:#ddd"></div>
		
	
</body>
</html>
```



#### Clova05_sentiment_controller.java







#### SentimentVO.java

이번에는 결과값을 간단하게 출력하는게 아니라 VO 를 만들어서 콜렉션 형태로 담아보겠습니다.

```java
package com.cali.clova.controller;

public class SentimentVO {
	private String content;
	private String sentiment;
	
	private double negative;
	private double positive;
	private double neutral;
	
	private int offset;
	private int length;
	
	private int highOffset;
	private int highLength;
	}
```



#### Clova05_sentiment_controller.java