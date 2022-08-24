# Naver Cloud Platform API : Voice

> Wed Aug 24, 2022

---

[toc]

[CLOVA Voice](https://api.ncloud-docs.com/docs/ai-naver-clovavoice-ttspremium)

#### home.jsp

```jsp
<li><a href="/clova/voiceForm">CLOVA Voice : 다양하고 자연스러운 목소리를 만들 수 있는 고품질 음성 합성 서비스</a></li>
```



#### Clova08_voice_controller.java

```java
package com.cali.clova.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class Clova08_voice_controller {

	@GetMapping("/clova/voiceForm")
	public String voiceForm() {
		return "clova/voiceForm";
	}
}
```



#### voiceForm.jsp

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
			#voiceText{
				width: 100%;
				height: 200px;
			}
		</style>
		<script>
			$(function(){
				$("#voiceBtn").click(function(){
					let xhr = new XMLHttpRequest(); // xhr 은 전체 페이지의 새로고침 없이도 URL 로부터 데이터를 받아올 수 있다.
					xhr.responseType = "blob"; // 응답받는 데이터 타입
					
					// 응답받은 경우 실행하는 곳
					xhr.onload = function(){
						var audioURL = URL.createObjectURL(this.response); // 오디오 정보가 있는 url을 객체로 만든다 // 여기서 response는 내장객체
						var audio = new Audio(); 
						audio.src = audioURL; // audioURL 에 있는 정보를 audio 로 넘겨주기
						audio.play(); // 재생
						
					};
					
					// 서버 매핑
					xhr.open("post", "/clova/voiceOk");
					xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
					xhr.send("text="+ $("#voiceText").val());
				});
			});
					
		</script>
	</head>
<body>
	<h2>CLOVA Voice</h2>
	<p>Premium API는 음성으로 변환할 텍스트와 음색, 속도, 감정 등을 파라미터로 입력받은 후 음성을 합성하여 그 결과를 반환하는 HTTP 기반의 REST API입니다.</p>
	
	<textarea id="voiceText">
	재규어 랜드로버 코리아(대표 로빈 콜건)가 23일 강원도 홍천군에서 랜드로버의 럭셔리 플래그십 SUV 올 뉴 레인지로버의 공식 출시 행사를 갖고 본격적인 판매에 돌입한다. 
	럭셔리 SUV의 시초인 레인지로버는 1970년 첫 선을 보인 후 우아한 디자인과 최상의 편안함과 여유로움, 그리고 모든 길을 정복할 수 있는 독보적인 주행성능으로 
	50년 이상 럭셔리 SUV 시장을 선도해왔다. 이번에 출시하는 올 뉴 레인지로버는 레인지로버 5세대로 모던 럭셔리 디자인, 혁신적인 테크놀로지와 최신 편의사양을 
	집약해 럭셔리 SUV의 새로운 기준을 제시한다.
	출처 : 모토야(https://auto.nate.com)
	</textarea><br/>
	<input type="button" value="음성으로 변환하기" id="voiceBtn"/>
</body>
</html>
```





#### Clova08_voice_controller.java

```java
package com.cali.clova.controller;

import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.File;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.net.URLEncoder;
import java.util.Date;

import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class Clova08_voice_controller {

	@GetMapping("/clova/voiceForm")
	public String voiceForm() {
		return "clova/voiceForm";
	}
	
	@PostMapping("/clova/voiceOk")
	@ResponseBody
	public void voiceOk(@RequestParam("text") String text, HttpServletResponse res) { // 클라이언트는 res 를 통해 데이터를 받는다.
		/////////////////
		String clientId = "";//애플리케이션 클라이언트 아이디값";
        String clientSecret = "";//애플리케이션 클라이언트 시크릿값";
        try {
            text = URLEncoder.encode(text, "UTF-8"); // 13자
            
            String apiURL = "https://naveropenapi.apigw.ntruss.com/tts-premium/v1/tts";
            URL url = new URL(apiURL);
            HttpURLConnection con = (HttpURLConnection)url.openConnection();
            con.setRequestMethod("POST");
            con.setRequestProperty("X-NCP-APIGW-API-KEY-ID", clientId);
            con.setRequestProperty("X-NCP-APIGW-API-KEY", clientSecret);
            // post request
            String postParams = "speaker=jinho&volume=0&speed=0&pitch=0&format=mp3&text=" + text; // ** 수정 가능
            con.setDoOutput(true);
            DataOutputStream wr = new DataOutputStream(con.getOutputStream());
            wr.writeBytes(postParams);
            wr.flush();
            wr.close();
            // -- 쓰기 완료
            
            int responseCode = con.getResponseCode();
            BufferedReader br;
            if(responseCode==200) { // 정상 호출
                InputStream is = con.getInputStream();
                int read = 0;
                byte[] bytes = new byte[1024];
                // 랜덤한 이름으로 mp3 파일 생성
                String tempname = Long.valueOf(new Date().getTime()).toString();
                File f = new File(tempname + ".mp3");
                f.createNewFile();
                OutputStream outputStream = new FileOutputStream(f); // 파일로 쓰기 더 정확하게는 FileOutputStream 
                
                // 클라이언트에게 다시 보내는 작업
                OutputStream os = res.getOutputStream();

                // --------------
                while ((read =is.read(bytes)) != -1) {
                    outputStream.write(bytes, 0, read); 
                    os.write(bytes, 0, read); // 읽어온 데이터는 bytes 단위로 되어있고
                }
                is.close();
                os.close();
                
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
		
		/////////////////
	}
}

```

