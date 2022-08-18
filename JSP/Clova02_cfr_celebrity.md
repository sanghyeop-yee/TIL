# Naver Cloud Platform API : CFR Celebrity

> Thu Aug 18, 2022

---

[toc]



이번에는 celebrity (유명인 얼굴 인식) API를 사용해봅시다.

먼저 Home.jsp 에서 링크를 추가해줍시다.



#### home.jsp

mapping 주소 = "/cloav/cfr_celebrity"

```jsp
<li><a href="/cloav/cfr_celebrity">CFR(Clova Face Recognition) : 유명인 얼굴인식</a></li>
```



#### Clova02_cfr_celebrity_controller

```java
package com.cali.clova.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class Clova02_cfr_celebrity_controller {
	@RequestMapping("/cloav/cfr_celebrity") // home.jsp 에서 매핑주소
	public String celebrity() {
		return "/cloav/cfr_celebrity"; // view 를 위한 jsp 생성
	}
}

```



#### cf_celebrity.jsp

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
	<h2>유명인 얼굴 인식 API</h2>
	<pre>
		입력받은 이미지로부터 얼굴을 감지하고 감지한 얼굴이 어떤 유명인과 닮았는지 분석하여 그 결과를 반환하는 REST API입니다. 이미지에서 다음과 같은 정보를 분석합니다.

			감지된 얼굴의 수
			감지된 각 얼굴을 분석한 정보
			닮은 유명인 이름
			해당 유명인을 닮은 정도
	</pre>
	<form method="post" enctype="multipart/form-data" id="fileupload"> <!-- ajax 로 호출시엔 action 이 필요없다. --> 
		이미지 선택 : <input type="file" name="image" id="image"/>
		<input type="submit" value="확인하기"/>
	</form>
	<textarea id="txt"></textarea>
</body>
</html>
```



이제 서버에서 확인해 봅니다.

확인이 되었으면 자바스크립트로 유효성 검사를 추가합니다. 

맵핑주소 "/clova/cfr_celebrity_ok"

```javascript
$(function(){
				$("#fileupload").submit(function(){
					event.preventDefault(); //
					
					if($("#image").val()==""){
						alert("이미지 파일을 선택하세요.");
						return;
					}
					// 이미지파일이 포함된 폼을 객체로 만들어 서버로 전송한다. 
					var data = new FormData($("#fileupload")[0]); 
					$.ajax({
						url : "/clova/cfr_celebrity_ok",
						type : "post",
						async : false,
						processData : false,
						contentType : false,
						data: data,
						dataType : "text",
						success : function(result){
							
						}, error : function(e){
							console.log(e.responseText);
						}
					});
				});
			});
```



#### Clova02_cfr_celebrity_controller

맵핑주소 "/clova/cfr_celebrity_ok" 를 RequestMapping 으로 지정해주고 file 의 경로를 설정하고 

```java
@RequestMapping(value="/clova/cfr_celebrity_ok", method=RequestMethod.POST) // 이미지첨부 때문에 POST 방식
	@ResponseBody // 리턴하는게 콘텐츠다
	public String celebrityOk(@RequestParam("image") MultipartFile file, HttpSession session) {
		String path = session.getServletContext().getRealPath("/file");
		
```

API 를 자바 클래스 형태로 가져오고 필요한 내용을 수정합니다.

```java
StringBuffer reqStr = new StringBuffer();
	        String clientId = "";//애플리케이션 클라이언트 아이디값";
	        String clientSecret = "";//애플리케이션 클라이언트 시크릿값";
	        StringBuffer response = new StringBuffer();

	        try {
	            String filename = ClovaFileupload.fileUpload(path, file);
	        	
	        	String paramName = "image"; // 파라미터명은 image로 지정
	            String imgFile = path + "/" + filename;
	            File uploadFile = new File(imgFile);
	            
	            String apiURL = "https://naveropenapi.apigw.ntruss.com/vision/v1/celebrity"; // 유명인 얼굴 인식
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
	            writer.flush(); // outputSteam 
	            
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
	                br = new BufferedReader(new InputStreamReader(con.getInputStream())); // inputStream
	            } else {  // 오류 발생
	                System.out.println("error!!!!!!! responseCode= " + responseCode);
	                br = new BufferedReader(new InputStreamReader(con.getInputStream()));
	            }
	            String inputLine;
	            if(br != null) {
	                // StringBuffer response = new StringBuffer();
	                while ((inputLine = br.readLine()) != null) {
	                    response.append(inputLine);
	                }
	                br.close();
	                System.out.println(response.toString());
	            } 
//	            else {
//	                System.out.println("error !!!");
//	            }
	        } catch (Exception e) {
	            System.out.println(e);
	        }
	        /////////////////////////////////////////////////
	        System.out.println("celebrity->"+ response.toString());
	        return response.toString(); // 클라이언트 쪽으로 정보 넘기기
}
```



#### cf_celebrity.jsp

ajax 를 이용하여 테이블로 출력해보겠습니다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>Insert title here</title>
		<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" integrity="sha384-xOolHFLEh07PJGoPkLv1IbcEPTNtaed2xpHsD9ESMhqIYd0nLMwNLD69Npy4HI+N" crossorigin="anonymous">
		<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
		<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-Fy6S3B9q64WdZWQUiU+q4/2Lc9npb8tCaSX9FK7E8HnRr0Jz8D6OP9dO5Vg3Q9ct" crossorigin="anonymous"></script>
		<style>
		</style>
		<script>
			$(function(){
				$("#fileupload").submit(function(){
					event.preventDefault(); //
					
					if($("#image").val()==""){
						alert("이미지 파일을 선택하세요.");
						return;
					}
					// 이미지파일이 포함된 폼을 객체로 만들어 서버로 전송한다. 
					var data = new FormData($("#fileupload")[0]); 
					$.ajax({
						url : "/clova/cfr_celebrity_ok",
						type : "post",
						async : false,
						processData : false,
						contentType : false,
						data: data,
						dataType : "text",
						success : function(result){
							$("#txt").val(result);
							
							// 문자열을 JSON 으로 변환
							var jsonData = JSON.parse(result);
							console.log(jsonData);
							
							// 테이블로 출력해보기
							var tag = "<table class='table table-dark'>";
							tag += "<tr><td>번호</td><td>닮은 연예인</td><td>정확도</td></tr>";
							
							// 사람수 만큼 반복문 실행
							jsonData.faces.map(function(f, i){
								tag += "<tr><td>"+(i+1)+"</td>";
								tag += "<td>" + f.celebrity.value + "</td>";
								tag += "<td>" + parseInt(f.celebrity.confidence*100) + "%</td></tr>";
								
							});
							
							tag += "</table>" // 테이블 닫기
							// $("#view").html(tag); // 출력
							console.log(tag);
							
							$("body").append(tag);
							
						}, error : function(e){
							console.log(e.responseText);
						}
					});
					
				});
			});
			
		</script>
	</head>
<body>
	<h2>유명인 얼굴 인식 API</h2>
	<pre>
		입력받은 이미지로부터 얼굴을 감지하고 감지한 얼굴이 어떤 유명인과 닮았는지 분석하여 그 결과를 반환하는 REST API입니다. 이미지에서 다음과 같은 정보를 분석합니다.

			감지된 얼굴의 수
			감지된 각 얼굴을 분석한 정보
			닮은 유명인 이름
			해당 유명인을 닮은 정도
	</pre>
	<form method="post" enctype="multipart/form-data" id="fileupload"> <!-- ajax 로 호출시엔 action 이 필요없다. --> 
		이미지 선택 : <input type="file" name="image" id="image"/>
		<input type="submit" value="확인하기"/>
	</form>
	<textarea id="txt" style="width:600px; height:300px;"></textarea>
	<div id="view"></div>
</body>
</html>
```

