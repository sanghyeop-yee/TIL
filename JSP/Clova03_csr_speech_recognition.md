# Naver Cloud Platform API : CSR Speech

> Thu Aug 18, 2022

----

[toc]

이번에는 사람의 목소리를 텍스트로 바꿔주는 음성 인식 API 를 활용해봅시다. 

[Clova Speech Recognition](https://guide.ncloud-docs.com/docs/ko/naveropenapiv3-speech-recognition-sdk)



#### home.jsp

전송을 하기 위한 링크 "/clova/csr_speech" 를 홈화면에 추가해줍니다.

```jsp
<li><a href="/clova/csr_speech">CSF(Clova Speech Recognition) : 음성을 텍스트로</a></li>
```



#### csr_speech.jsp

csf_speech 를 작동시킬 뷰페이지를 만들어줍니다. 

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
		<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
		<style>
		</style>
		<script>
		</script>
	</head>
<body>
	<h2>가장 뛰어난 한국어 음성 인식률을 가진 음성 인식 API</h2>
	<p>
	사람의 목소리를 인식하여 작동하는 비서 애플리케이션, 챗봇, 음성 메모 등의 서비스를 만들 때 활용할 수 있는 음성 인식 API 서비스입니다. 
	음성 데이터는 API를 통해 CLOVA Speech Recognition(CSR) 엔진으로 전송되며, 해당 음성 데이터를 인식해서 텍스트로 변환하여 전달해줍니다.
	</p>
</body>
</html>
```



#### Clova03_csr_speech_controller

클라이언트가 음성파일을 로컬서버로 업로드 > 서버에서 ncp 로 > ncp 에서 서버로 응답 > 서버에서 클라이언트로 응답하는 구조입니다. 

```java
package com.cali.clova.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class Clova03_csr_speech_controller {
	
	@RequestMapping("/clova/csr_speech")
	public String csr_speech() {
		return "clova/csr_speech";
	}
}

```

home 에서 링크를 확인합니다.



#### csr_speech.jsp

음성파일을 선택하여 업로드할 폼을 추가해줍니다.

```jsp
	<form method="post" enctype="multipart/form-data" id="csrForm">
		음성파일 선택 : <input type="file" name="audio" id="audio"/>
		<input type="submit" value="시작"/>
	</form>
	<textarea id="csrResult"></textarea>
	<div id="csrTxt"></div>
```



/clova/csr_speech_ok 로 보내주는 자바스크립트를 추가합니다.

```javascript
		<script>
			$(function(){
				$(document).on('submit', '#csrForm', function(){
					event.preventDefault(); // 기본이벤트 제거
					//mp3, aac, ac3, ogg, flac, wav만 지원가능
					var filename = $("#audio").val();
					// console.log(filename);
					if(filename!=""){
						let point = filename.lastIndexOf(".");
						let extension = filename.substring(point+1);
						
						// 지원하는 음성파일인 경우
						if(extension=='mp3' || extension=='acc' || extension=='ac3' || extension=='ogg' || extension=='flac' || extension=='wav'){
							var formObj = new FormData($("#csrForm")[0]);
							var url = "/clova/csr_speech_ok";
							
							$.ajax({
								type: 'post',
								async: false,
								processData: false,
								contentType: false,
								data: formObj,
								dataType: "text",
								success: function(result, status){
									
								}, error: function(e){
									console.log(e.responseText);
								}
							});
						}else{
							alert("음성파일이 아닙니다.");
							return false;
						}
					}else{
						alert("음성파일을 선택하세요.");
						return false;
					}
					
				
					
					
				});
			});
		</script>
```



#### Clova03_csr_speech_controller

/clova/csr_speech_ok 를 맵핑받는 컨트롤러를 작성합니다.

[speech to text](https://api.ncloud-docs.com/docs/ai-naver-clovaspeechrecognition-stt)

```java
	@RequestMapping(value="/clova/csr_speech_ok", method=RequestMethod.POST)
	public String csrSpeechOk(@RequestParam("audio") MultipartFile audio) {
		
	}
```



```java
package com.cali.clova.controller;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileInputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;

import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.multipart.MultipartFile;

@Controller
public class Clova03_csr_speech_controller {
	
	@RequestMapping("/clova/csr_speech")
	public String csr_speech() {
		return "clova/csr_speech";
	}
	
	@RequestMapping(value="/clova/csr_speech_ok", method=RequestMethod.POST)
	public String csrSpeechOk(@RequestParam("audio") MultipartFile audio, HttpServletRequest request) {
		String path = request.getServletContext().getRealPath("/file");
		
		String clientId = "";             // Application Client ID";
        String clientSecret = "";     // Application Client Secret";
        StringBuffer response = new StringBuffer();

        try {
        	String filename = ClovaFileupload.fileUpload(path, audio);
        	
            String imgFile = path + "/" + filename;
            File voiceFile = new File(imgFile);

            String language = "Kor";        // 언어 코드 ( Kor, Jpn, Eng, Chn )
            String apiURL = "https://naveropenapi.apigw.ntruss.com/recog/v1/stt?lang=" + language;
            URL url = new URL(apiURL);

            HttpURLConnection conn = (HttpURLConnection)url.openConnection();
            conn.setUseCaches(false);
            conn.setDoOutput(true);
            conn.setDoInput(true);
            conn.setRequestProperty("Content-Type", "application/octet-stream");
            conn.setRequestProperty("X-NCP-APIGW-API-KEY-ID", clientId);
            conn.setRequestProperty("X-NCP-APIGW-API-KEY", clientSecret);

            OutputStream outputStream = conn.getOutputStream();
            FileInputStream inputStream = new FileInputStream(voiceFile);
            byte[] buffer = new byte[4096];
            int bytesRead = -1;
            while ((bytesRead = inputStream.read(buffer)) != -1) {
                outputStream.write(buffer, 0, bytesRead);
            }
            outputStream.flush();
            inputStream.close(); // 결과가 돌아오는 곳
            
            BufferedReader br = null;
            int responseCode = conn.getResponseCode();
            if(responseCode == 200) { // 정상 호출
                br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            } else {  // 오류 발생
                System.out.println("error!!!!!!! responseCode= " + responseCode);
                br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            }
            String inputLine;

            if(br != null) {
                //StringBuffer response = new StringBuffer();
                while ((inputLine = br.readLine()) != null) {
                    response.append(inputLine);
                }
                br.close();
                System.out.println(response.toString());
            }
        } catch (Exception e) {
            System.out.println(e);
        }
        //
        System.out.println("speech->"+ response.toString());
        return response.toString();
	}
}

```

