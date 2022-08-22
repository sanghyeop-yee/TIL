

# Naver Cloud Platform API : CSR Speech Record

> Thu Aug 18, 2022

[toc]

이어서 녹음를 해서 바로 음성을 텍스트로 변환하는 기능을 만들어봅시다.

녹음 기능은 자바스크립트의 객체 Navigation 이 합니다.



 녹음 | 정지 



#### [View] home.jsp

홈화면에 링크 추가하기

```jsp
<li><a href="/clova/csr_speech_record">CSF(Clova Speech Recognition) : 음성을 텍스트로 (음성녹음기능)</a></li>
```



#### [Controller] Clova03_csr_speech_record_controller.java

링크를 누르면 요청하여 리턴할 뷰페이지를 추가합니다.

```java
package com.cali.clova.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class Clova03_csr_speech_record_controller {
	
	@RequestMapping("/clova/csr_speech_record")
	public String csrRecord() {
		return "clova/csr_speech_record"; // 뷰페이지
	}
}

```



#### [View] csr_speech_record.jsp

녹음 기능을 jquery 말고 javascript 로 구현해보겠습니다.

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
			// javascript
			function mediaStart(){
				var record = document.getElementById("record"); // button 의 id record 를 의미
				var stop = document.getElementById("stop"); // button 의 id stop 을 의미
				var result = document.getElementById("result");
			}
		</script>
	</head>
<body onload="mediaStart()">
	<h1>녹음파일에 대한 CSR</h1>
	<input type="button" id="record" value="녹음시작"/>
	<input type="button" id="stop" value="녹음정지"/><br/>
	<div id="result"></div>
</body>
</html>
```



이제 홈화면으로 가서 링크를 클릭하면 뷰페이지가 나오는지 확인합니다.



#### [Controller] Clova03_csr_speech_record_controller.java

