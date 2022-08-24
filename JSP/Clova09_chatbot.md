# Naver Cloud Platform API : Chatbot

> Wed Aug 24, 2022

---



챗봇을 사용하기 위해서는 시나리오가 필요합니다.

[CLOVA Chatbot](https://www.ncloud.com/product/aiService/chatbot)

[정규식 입력방법](https://guide.ncloud-docs.com/docs/ko/chatbot-chatbot-3-7)

[CLOVA Chatbot API Guide](https://api.ncloud-docs.com/docs/ai-application-service-chatbot)



콘솔로 들어가서 대화목록을 추가합니다.

![image-20220824151146639](../../Clova09_chatbot.assets/image-20220824151146639.png)



답변으로 이미지 옵션을 추가할 수 있습니다.

![image-20220824151208297](../../Clova09_chatbot.assets/image-20220824151208297.png)



Console > API Gateway 로 가서 API Keys 를 하나 생성하고

다시 빌더로 돌아와서 메신저 연동 > Custom > 자동연동을 설정합니다. 



#### home.jsp

```jsp
<li><a href="/clova/chatbotForm">CLOVA Chatbot : 마케팅, 고객 응대 등 다양한 서비스에 활용할 수 있는 챗봇을 생성하는 서비스</a></li>
```



#### Clova09_chatbot_controller

```java
package com.cali.clova.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class Clova09_chatbot_controller {
	
	@GetMapping("/clova/chatbotForm")
	public String chatbotForm() {
		return "clova/chatbotForm";
	}
	
	
}

```



#### chatbotForm.jsp

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
				$("#query").click(function(){
					if($("#queryin").val()!=""){ // 질문이 있을떄
						
						$.ajax({
							type: "post",
							dataType: "text",
							async: false,
							url: "/clova/chatbotOk",
							data: {
								queryin: $("#queryin").val()
							}, success: function(result){
								
								$("#jsonCode").val(result);
								
							}, error: function(){
								console.log(e.responseText);
							}
						
						});
					}
					
				});
			});
		</script>
	</head>
<body>
<h2>Chatbot</h2>
<div id="content" style="width:100%; height:400px; border:1px solid #ddd;"></div>
<input type="text" name="queryin" id="queryin"/>
<input type="button" value="query" id="query"/>
<hr/>
<textarea id="jsonCode" style="width:100%; height:300px;"></textarea>
</body>
</html>
```



#### Clova09_chatbot_controller

```java
```

