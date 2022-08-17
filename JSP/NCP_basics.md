# Naver Cloud Platform API

> Wed Aug 17

Naver Cloud Platform 을 이용하여 AI 서비스를 개발해봅시다.

Controller (자바 IO) 를 이용해서 네이버에 필요한 정보를 보내고 돌려받아서 클라이언트에게 보내주는게 한 사이클입니다.

이것이 바로 REST API 입니다. 

처음에 [CFR](https://www.ncloud.com/product/aiService/cfr) 이라는 API 를 활용해봅시다.



아래의 계정으로 로그인합니다.

> https://www.ncloud.com/nsa/aiplatform19th3
>
> 



AI Naver API > Application 등록 
Application 이름 설정 > 서비스 선택 >  Web 서비스 URL (http://localhost) > 등록



STS 를 통해 Ajax 로 구현해봅시다.

새로운 Spring Starter Project 를 만들고 아래의 Dependencies 를 선택해줍니다.

> Spring Boot DevTools, Spring Web



### 사용자화면 만들기

우선 프론트단을 구성해봅시다. 

src > main > webapp > WEB-INF > views 폴더를 생성해줍니다.



다음은 build.grade 에 프레임워크를 추가하고 refresh gradle project 를 해줍니다. 메이븐 저장소로 이동하여 Jasper, JSTL, [JSON In Java](https://mvnrepository.com/artifact/org.json/json), [Android Utilities](https://mvnrepository.com/artifact/net.morimekta.utils/android-util) 를 build.gradle 에 추가해줍니다.

```xml
// view를 jsp로 사용하기 위해서 framework 추가
	// https://mvnrepository.com/artifact/org.apache.tomcat.embed/tomcat-embed-jasper
	implementation group: 'org.apache.tomcat.embed', name: 'tomcat-embed-jasper', version: '9.0.58'
	// https://mvnrepository.com/artifact/javax.servlet/jstl
	implementation group: 'javax.servlet', name: 'jstl', version: '1.2'
// JSON in Java
	// https://mvnrepository.com/artifact/org.json/json
	implementation 'org.json:json:20220320'
	
	// https://mvnrepository.com/artifact/net.morimekta.utils/android-util
	implementation 'net.morimekta.utils:android-util:3.0.1'
```



다음은 application.properties 를 설정합니다.

```xml
server.port=8050

spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp

# 한글 encoding
spring.mandatory-file-encoding=UTF-8
spring.servlet.encoding.charset=UTF-8
spring.servlet.encoding.enabled=true

```



preference 로 이동하여 웹이 UTF-8 로 설정이 되어 있는지 확인합니다. 



Views > home.jsp 를 하나 만듭니다.

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
	<h1>Clova API 홈</h1>
	<ol>
		<li><a href="/clova/cfr">CFR(Clova Face Recognition) : 얼굴감지(눈,코,입,나이,얼굴방향,표정)</a></li>
	</ol>
</body>
</html>
```



src/main/java > com.cali.clova 에 HomeController.java 를 생성하고 서버 연결을 확인합니다.

```java
package com.cali.clova;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HomeController {
	@RequestMapping("/")
	public String home() {
		return "home";
	}
}

```



링크("/clova/cfr") 를 걸어주고 clova 뷰를 모아줄 views>clova 폴더를 생성하고 com.cali.clova > com.cali.clova.controller 패키지를 생성해줍니다. 이어서 Clova01_cfr_recognition_controller.java 를 만들어줍니다. (원래 클래스는 camel 표기법이지만 여기서는 가독성을 위해)

```java
package com.cali.clova.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class Clova01_cfr_recognition_controller {
	
	// 폼
	@RequestMapping("/clova/cfr")
	public String cfr() {
		return "clova/cfr_recognition";
	}
}

```



`return "clova/cfr_recognition";`  을 하기 때문에 clova 뷰페이지에 clova > `cfr_recognition.jsp` 를 생성합니다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
		<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
		<link rel="stylesheet" href="/js_css/style.css" type="text/css"/>
		<style>
		</style>
		<script>
		</script>
	</head>
<body>
	<div>
		<h1>얼굴과 관련된 다양한 정보를 제공하는 얼굴 인식 API</h1>
		<p>입력된 비전 데이터를 통해 얼굴을 인식하거나 얼굴 감지를 이용한 애플리케이션을 만들 때 유용한 API 서비스입니다. 
		이미지 속의 얼굴과 가장 닮은 유명인을 찾거나, 얼굴의 윤곽과 눈/코/입 위치, 표정 값을 얻을 수 있습니다. </p>
		<hr/>
		<form method="post" action="/cfrOk" enctype="multipart/form-data">
			이미지파일선택 : <input type="file" id="image" name="image"/><br/>
			<button id="cfr">확인하기</button>
		</form>
		<hr/>
		<textarea id="txt" rows="20" cols="100"></textarea>
		
	</div>
</body>
</html>
```



이미지를 선택안하면 확인하기를 할 수 없도록 Ajax 로 함수를 작성하겠습니다.

```javascript
		<script>
			$(function(){
				$("#cfrForm").submit(function(){
					event.preventDefault(); // 기본이벤트 제거
					
					if($("#image").val()==""){
						alert("이미지를 선택하세요.");
						return false;
					}else{
						///
					}
					
				});
			});
		</script>
```





### 서버

하나는 우리의 로컬 서버, 하나는 네이버 서버입니다. 파일을 선택하고 submit 을 하면 우리의 로컬 서버로 넘어오게 됩니다. 이떄 MultipartFile 를 사용합니다. 파일명 뿐 아니라 내용까지도 넘겨야 하겠죠?

폼객체안에 파일이 담겨져 있습니다. 

form 의 `action="/cfrOk"` 을 지우고 ajax 의 url 로 만들겠습니다. 

이어서 api 를 가져오겠습니다. 

```java
StringBuffer reqStr = new StringBuffer();
        String clientId = "YOUR_CLIENT_ID";//애플리케이션 클라이언트 아이디값";
        String clientSecret = "YOUR_CLIENT_SECRET";//애플리케이션 클라이언트 시크릿값";

        try {
            String paramName = "image"; // 파라미터명은 image로 지정
            String imgFile = "이미지 파일 경로 ";
            File uploadFile = new File(imgFile);
            String apiURL = "https://naveropenapi.apigw.ntruss.com/vision/v1/face"; // 얼굴 감지
            URL url = new URL(apiURL);
            HttpURLConnection con = (HttpURLConnection)url.openConnection();
            con.setUseCaches(false);
            con.setDoOutput(true);
            con.setDoInput(true);
            // multipart request
            String boundary = "---" + System.currentTimeMillis() + "---";
            con.setRequestProperty("Content-Type", "multipart/form-data; boundary=" + boundary);
            con.setRequestProperty("X-NCP-APIGW-API-KEY-ID", clientId);
            con.setRequestProperty("X-NCP-APIGW-API-KEY", clientSecret);
            OutputStream outputStream = con.getOutputStream();
            PrintWriter writer = new PrintWriter(new OutputStreamWriter(outputStream, "UTF-8"), true);
            String LINE_FEED = "\r\n";
            // file 추가
            String fileName = uploadFile.getName();
            writer.append("--" + boundary).append(LINE_FEED);
            writer.append("Content-Disposition: form-data; name=\"" + paramName + "\"; filename=\"" + fileName + "\"").append(LINE_FEED);
            writer.append("Content-Type: "  + URLConnection.guessContentTypeFromName(fileName)).append(LINE_FEED);
            writer.append(LINE_FEED);
            writer.flush();
            FileInputStream inputStream = new FileInputStream(uploadFile);
            byte[] buffer = new byte[4096];
            int bytesRead = -1;
            while ((bytesRead = inputStream.read(buffer)) != -1) {
                outputStream.write(buffer, 0, bytesRead);
            }
            outputStream.flush();
            inputStream.close();
            writer.append(LINE_FEED).flush();
            writer.append("--" + boundary + "--").append(LINE_FEED);
            writer.close();
            BufferedReader br = null;
            int responseCode = con.getResponseCode();
            if(responseCode==200) { // 정상 호출
                br = new BufferedReader(new InputStreamReader(con.getInputStream()));
            } else {  // 오류 발생
                System.out.println("error!!!!!!! responseCode= " + responseCode);
                br = new BufferedReader(new InputStreamReader(con.getInputStream()));
            }
            String inputLine;
            if(br != null) {
                StringBuffer response = new StringBuffer();
                while ((inputLine = br.readLine()) != null) {
                    response.append(inputLine);
                }
                br.close();
                System.out.println(response.toString());
            } else {
                System.out.println("error !!!");
            }
        } catch (Exception e) {
            System.out.println(e);
        }
```



다음으로 webapp 밑에 file 폴더를 생성합니다. 업로드할 위치 경로를 지정하겠습니다. 

이번에는 파일 업로드하는 클래스를 따로 만들어 보겠습니다.

com.campus.clov.controller 에 CloverFileupload.java 클래스를 생성합니다.

```java
package com.cali.clova.controller;

import java.io.File;

import org.springframework.web.multipart.MultipartFile;

public class ClovaFileupload {
	public static String fileUpload(String path, MultipartFile f) { // 경로와 MultipartFile 를 매개로 받는다. 
		// 파일명을 리턴함
		String orgFilename = f.getOriginalFilename();
		
		try {
			f.transferTo(new File(path, orgFilename));
		}catch(Exception e) {
			e.printStackTrace();
		}
		return orgFilename;
	}
}

```









