# Naver Cloud Platform API : Captcha

> Mon Aug 22, 2022

---

[toc]



#### home.jsp

링크 추가

```jsp
<li><a href="/clava/captcha">CAPTCHA(image) : 이미지 캡차 API는 자동 입력 방지를 위해 사람의 눈으로 식별 가능한 문자가 포함된 이미지를 전송하고 입력값을 검증하는 REST API</a></li>
```



#### Clova06_captcha_image_controller.java

```java
package com.cali.clova.controller;

import org.springframework.stereotype.Controller;

@Controller
public class Clova06_captcha_image_controller {
	public String captchaForm() {
		return "clova/captchaForm_img";
	}
}
```



#### captchaForm_img.jsp

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
		</script>
	</head>
<body>
	<h2>Captcha(Image)</h2>
	<p>
		이미지 캡차 API는 자동 입력 방지를 위해 사람의 눈으로 식별 가능한 문자가 포함된 이미지를 전송하고 입력값을 검증하는 REST API입니다.
		비로그인 오픈 API 이므로 GET으로 호출할 때 HTTP 헤더에 애플리케이션 등록 시 발급받은 Client ID와 Client Secret 값을 같이 전송해 활용할 수 있습니다.
		캡차 기능 구현 절차는 다음과 같습니다.
		캡차 API에는 캡차키 발급/비교 API와 캡차 이미지 요청 API가 있습니다. 
		캡차키 발급/비교 API는 아래의 1, 3 번 기능을 제공하고 캡차 이미지 요청 API는 2 번 기능을 제공합니다.
		
		<pre>
		1. 캡차키를 발급 요청하여 발급받습니다.
		2. 발급받은 캡차키를 이용해 캡차 이미지를 요청하여 발급받습니다.
		3. 사용자가 이미지를 보고 입력한 값을 캡차키와 비교합니다.
		</pre>
	</p>
	<hr/>
	<!-- 사용자에게 표시할 캡차이미지 -->
	<img src=""/><br/>
	<form method="post" action="">
		<input type="text" name="userin"/>
		<input type="submit" value="전송하기"/>
	</form>
</body>
</html>
```



#### Clova06_captcha_image_controller.java

```java
package com.cali.clova.controller;

import org.springframework.stereotype.Controller;

@Controller
public class Clova06_captcha_image_controller {
	public String captchaForm() {
		return "clova/captchaForm_img";
	}
}
```





```java
package com.cali.clova.controller;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;

import java.nio.charset.Charset;
import java.util.Date;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.json.JSONObject;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class Clova06_captcha_image_controller {
	
	String clientId = "";
	String clientSecret = "";
	
	@GetMapping("/clova/captchaForm")
	public String captchaForm() {
		return "clova/captchaForm_img";
	}
	
	String key;
	// 캡차 이미지 수신
	@RequestMapping("/clova/captchaImage")
	public void captchaImage(HttpServletRequest request, HttpServletResponse res) { // 임의대로 메소드 생성
		String path = request.getServletContext().getRealPath("/file");
		//////////////////////////////////////////////////////////
	        try {
	        	key = captchaKey(); // https://naveropenapi.apigw.ntruss.com/captcha/v1/nkey 호출로 받은 키값
	        	
	            String apiURL = "https://naveropenapi.apigw.ntruss.com/captcha-bin/v1/ncaptcha?key=" + key + "&X-NCP-APIGW-API-KEY-ID" + clientId;
	            URL url = new URL(apiURL);
	            HttpURLConnection con = (HttpURLConnection)url.openConnection();
	            con.setRequestMethod("GET");
	            
	            int responseCode = con.getResponseCode();
	            BufferedReader br;
	            if(responseCode==200) { // 정상 호출
	                InputStream is = con.getInputStream(); // 데이터를 받음
	                int read = 0;
	                byte[] bytes = new byte[1024];
	                // 랜덤한 이름으로 파일 생성
	                String tempname = Long.valueOf(new Date().getTime()).toString();
	                File f = new File(tempname + ".jpg"); // 확장자를 포함시켜서
	                f.createNewFile(); //파일을 생성하겠다 저장은 위의 String path 로
	                OutputStream fileOutputStream = new FileOutputStream(f);
	                OutputStream outputStream = res.getOutputStream();
	                
	                while ((read =is.read(bytes)) != -1) {
	                	fileOutputStream.write(bytes, 0, read); // 파일로 쓰기
	                	outputStream.write(bytes, 0, read); // 클라이언트에게 쓰기
	                	
	                }
	                is.close();
	                fileOutputStream.close();
	                outputStream.close();
	                
	            } else {  // 오류 발생
	                br = new BufferedReader(new InputStreamReader(con.getErrorStream()));
	                String inputLine;
	                StringBuffer response = new StringBuffer();
	                while ((inputLine = br.readLine()) != null) {
	                    response.append(inputLine);
	                }
	                br.close();
	                System.out.println(response.toString());
	            }
	        } catch (Exception e) {
	            System.out.println(e);
	        }
	   //////////////////////////////////////////////////////////
	}
	// 캡차 키값 비교
	@PostMapping("/clova/captchaCheck")
	public ResponseEntity<String> captchaCheck(@RequestParam("userIn") String userIn) {
		//////////////
		StringBuffer response = new StringBuffer();
		try {
            String code = "1"; // 키 발급시 0,  캡차 이미지 비교시 1로 세팅
            // String key = "CAPTCHA_KEY"; // 캡차 키 발급시 받은 키값
            String value = userIn; // 사용자가 입력한 캡차 이미지 글자값
            String apiURL = "https://naveropenapi.apigw.ntruss.com/captcha/v1/nkey?code=" + code +"&key="+ key + "&value="+ value;

            URL url = new URL(apiURL);
            HttpURLConnection con = (HttpURLConnection)url.openConnection();
            con.setRequestMethod("GET");
            con.setRequestProperty("X-NCP-APIGW-API-KEY-ID", clientId);
            con.setRequestProperty("X-NCP-APIGW-API-KEY", clientSecret);
            
            int responseCode = con.getResponseCode();
            BufferedReader br;
            if(responseCode==200) { // 정상 호출
                br = new BufferedReader(new InputStreamReader(con.getInputStream()));
            } else {  // 오류 발생
                br = new BufferedReader(new InputStreamReader(con.getErrorStream()));
            }
            String inputLine;
            
            
            while ((inputLine = br.readLine()) != null) {
                response.append(inputLine);
            }
            br.close();
            
            System.out.println(response.toString());
        } catch (Exception e) {
            System.out.println(e);
        }
		
		//////////////
		JSONObject jsonObj = new JSONObject(response.toString());
		
		ResponseEntity<String> entity = null;
		HttpHeaders headers = new HttpHeaders();
		headers.setContentType(new MediaType("text", "html", Charset.forName("UTF-8")));
		headers.add("Content-Type", "text/html; charset=UTF-8");
		
		String message = "";
		if(jsonObj.getBoolean("result")) { // 문자가 맞을 경우
			message += "<script>";
			message += "alert('홈으로 이동합니다.');";
			message += "location.href='/'";
			message += "</script>";
			entity = new ResponseEntity<String>(message, headers, HttpStatus.OK);
		}else { // 문자가 틀릴 경우
			message += "<script>";
			message += "alert('입력한 문자가 잘못되었습니다');";
			message += "location.href='/clova/captchaForm'";
			message += "</script>";
			entity = new ResponseEntity<String>(message, headers, HttpStatus.BAD_REQUEST);
		}
		
		return entity;
	}
	
	// 캡차 키 발급
	public String captchaKey() {
		//////////////////////////////////////////////////////////
		StringBuffer response = new StringBuffer();
		  try {
	            String code = "0"; // 키 발급시 0,  캡차 이미지 비교시 1로 세팅
	            String apiURL = "https://naveropenapi.apigw.ntruss.com/captcha/v1/nkey?code=" + code;
	            URL url = new URL(apiURL);
	            HttpURLConnection con = (HttpURLConnection)url.openConnection();
	            con.setRequestMethod("GET");
	            con.setRequestProperty("X-NCP-APIGW-API-KEY-ID", clientId);
	            con.setRequestProperty("X-NCP-APIGW-API-KEY", clientSecret);
	            int responseCode = con.getResponseCode();
	            BufferedReader br;
	            if(responseCode==200) { // 정상 호출
	                br = new BufferedReader(new InputStreamReader(con.getInputStream()));
	            } else {  // 오류 발생
	                br = new BufferedReader(new InputStreamReader(con.getErrorStream()));
	            }
	            String inputLine;
	            
	            while ((inputLine = br.readLine()) != null) {
	                response.append(inputLine);
	            }
	            br.close();
	            System.out.println(response.toString());
	        } catch (Exception e) {
	            System.out.println(e);
	        }
	//////////////////////////////////////////////////////////
		  JSONObject jsonObj = new JSONObject(response.toString());
		  String key = jsonObj.getString("key");
		  return key;
	}
}

```

