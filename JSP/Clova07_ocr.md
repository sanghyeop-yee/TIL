# Naver Cloud Platform API : OCR

> Mon Aug 22, 2022

---

[toc]

[CLOVA OCR](https://www.ncloud.com/product/aiService/ocr) / [CLOVA OCR Custom API](https://api.ncloud-docs.com/docs/ai-application-service-ocr-ocr)

[API Gateway](https://www.ncloud.com/product/applicationService/apiGateway) - gateway 필요



#### home.jsp

```jsp
<li><a href="/clova/ocrForm">OCR : 인쇄물 속 글자를 추출하여 디지털 데이터로 변환해 주는 서비스 (gateway 필요함)</a></li>
```





#### Clova07_ocr_text_controller.java

```java
package com.cali.clova.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class Clova07_ocr_text_controller {
	
	@RequestMapping("/clova/ocrForm")
	public String ocrForm() {
		return "clova/ocrForm";
	}
	
	

}

```





#### ocrForm.jsp

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
				$("#ocrForm").submit(function(){
					event.preventDefault(); // ocrForm 이벤트가 발생하면 기본이벤트를 제거하고
					const image = $("#image")[0]; // 이미지 파일 구하기
					if(image.files.length == 0){
						alert("텍스트가 있는 이미지 파일을 선택하세요.");
						return false;
					}
					
					// 이미지가 있을 경우
					var formData = new FormData($("#ocrForm")[0]);
					
					$.ajax({
						type: "post",
						dataType: "text",
						async: false,
						url: "/clova/ocrOk",
						processData: false,
						contentType: false,
						data: formData,
						success: function(result){
							
						}, error: function(error){
							console.log(error.responseText);
						}
					});
					
				});
			});
		</script>
	</head>
<body>
	<h2>CLOVA OCR: 이미지 속 문자 추출하여 컴퓨터 데이터로 변환</h2>
	<p>
	복잡하고 다양한 문서나 이미지 속 문자를 추출하여 데이터화하고 관리할 수 있는 서비스입니다.
	</p>
	<form method="post" enctype="multipart/form-data" id="ocrForm">
		텍스트가 있는 이미지 : <input type="file" name="image" id="image"/>
		<input type="submit" value="전송하기"/>
	</form>
	<hr/>
	<textarea id="result" style="width:100%; height:200px;"></textarea>
</body>
</html>
```





#### Clova07_ocr_text_controller

```java
package com.cali.clova.controller;

import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.UUID;

import org.json.JSONArray;
import org.json.JSONObject;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.multipart.MultipartFile;

@Controller
public class Clova07_ocr_text_controller {
	
	@RequestMapping("/clova/ocrForm")
	public String ocrForm() {
		return "clova/ocrForm";
	}
	
	@PostMapping("/clova/ocrOk")
	@ResponseBody // ajax 로 부르고 있기 때문에 필요
	public String ocrOk(@RequestParam("image") MultipartFile file) {
		//////////////////////////////////////////////////////////
		String apiURL = ""; // ocr 에서 제공하는 url 주소
		
		String secretKey = ""; 
		
		// String imageFile = "YOUR_IMAGE_FILE";
		StringBuffer response = new StringBuffer(); // json 이 문자로 저장되어있다.
		StringBuffer resultText = new StringBuffer(); // inferText 의 값을 문장으로 저장하기 위한 객체

		try {
			URL url = new URL(apiURL);
			HttpURLConnection con = (HttpURLConnection)url.openConnection();
			con.setUseCaches(false);
			con.setDoInput(true);
			con.setDoOutput(true);
			con.setReadTimeout(30000);
			con.setRequestMethod("POST");
			String boundary = "----" + UUID.randomUUID().toString().replaceAll("-", ""); // 임의의 16진수 문자열 생성
			System.out.println("UUID->"+ boundary); // 문자열 확인해보기
			
			con.setRequestProperty("Content-Type", "multipart/form-data; boundary=" + boundary); // 폼데이터로 보내야한다.
			con.setRequestProperty("X-OCR-SECRET", secretKey);

			JSONObject json = new JSONObject();
			json.put("version", "V2");
			json.put("requestId", UUID.randomUUID().toString());
			json.put("timestamp", System.currentTimeMillis());
			JSONObject image = new JSONObject();
			image.put("format", "jpg");
			image.put("name", "demo");
			JSONArray images = new JSONArray();
			images.put(image);
			json.put("images", images);
			String postParams = json.toString();

			con.connect();
			DataOutputStream wr = new DataOutputStream(con.getOutputStream());
			long start = System.currentTimeMillis();
			// File file = new File(imageFile);
			writeMultiPart(wr, postParams, file, boundary); // writeMultiPart 메소드를 필요 (api 로 가서 복사해오기)
			wr.close();

			int responseCode = con.getResponseCode();
			BufferedReader br;
			if (responseCode == 200) {
				br = new BufferedReader(new InputStreamReader(con.getInputStream()));
			} else {
				br = new BufferedReader(new InputStreamReader(con.getErrorStream()));
			}
			String inputLine;
			
			while ((inputLine = br.readLine()) != null) {
				response.append(inputLine);
			}
			br.close();

			System.out.println(response);
		} catch (Exception e) {
			System.out.println(e);
		}
		//////////////////////////////////////////////////////////
		System.out.println("ocr_response-->"+ response.toString());
		//////////////////////////////////////////////////////////
		// JSON 문자열을 JSON 객체로 만들어 필요한 문자만 구하여 문장을 만든다.
		JSONObject jsonObj = new JSONObject(response.toString());
		JSONArray arr = jsonObj.getJSONArray("images"); // images 를 꺼낸다. 
		JSONObject imageArr = arr.getJSONObject(0); // 여기서  
		
		JSONArray fieldArr = imageArr.getJSONArray("fields"); // fields 를 꺼낸다.
		for(int i=0; i<fieldArr.length(); i++) {
			JSONObject field = fieldArr.getJSONObject(i);
			field.getString("inferText"); // 키워드가 inferText
			resultText.append(field.getString("inferText")+" "); // 위에 선언한 resultText 에 붙이기
			if(field.getBoolean("lineBreak")) { // linebreak 에 따라 줄바꾸기
				resultText.append("\n");
			}
			
		}
		//////////////////////////////////////////////////////////
		return resultText.toString();
	}
	
	//---------------------
	private static void writeMultiPart(OutputStream out, String jsonMessage, MultipartFile file, String boundary) throws
		IOException {
		StringBuilder sb = new StringBuilder();
		sb.append("--").append(boundary).append("\r\n");
		sb.append("Content-Disposition:form-data; name=\"message\"\r\n\r\n");
		sb.append(jsonMessage);
		sb.append("\r\n");
	
		out.write(sb.toString().getBytes("UTF-8"));
		out.flush();
	
		if (!file.isEmpty()) { // **
			out.write(("--" + boundary + "\r\n").getBytes("UTF-8"));
			StringBuilder fileString = new StringBuilder();
			fileString
				.append("Content-Disposition:form-data; name=\"file\"; filename=");
			fileString.append("\"" + file.getName() + "\"\r\n");
			fileString.append("Content-Type: application/octet-stream\r\n\r\n");
			out.write(fileString.toString().getBytes("UTF-8"));
			out.flush();
			
			out.write(file.getBytes());
			out.write("\r\n".getBytes());
			out.write(("--" + boundary + "--\r\n").getBytes("UTF-8"));
			
		}
		out.flush();
	}
	//---------------------
	
}

```

