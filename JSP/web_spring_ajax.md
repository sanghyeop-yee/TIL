# spring 에서 ajax 구현하기

> Wed Jul 27, 2022

---

[toc]

화면전환없이 서버에 가서 정보를 가져오는 방법을 알아봅시다. 

서버에 가서 정보를 가져오기 위해 프레임워크 (메이븐 저장소) 가 필요합니다.

새로운 프로젝트를 Spring MVC Project 로, 패키지는 `com.cali.home` 로 만듭니다. 



메이븐 저장소에서 gson 프레임워크를 복사해옵니다.

프레임워크 추가는 pom.xml 에서 합니다. 이렇게 pom.xml 에 추가하는 것을 메이븐 방식이라고 합니다.

```xml
		<!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
		<dependency>
		    <groupId>com.google.code.gson</groupId>
		    <artifactId>gson</artifactId>
		    <version>2.9.0</version>
		</dependency>  
```



## 문자열 받아오기

views 페이지부터 설정합니다. ajax 라는 폴더를 만들고 ajaxView.jsp 파일을 생성합니다.



화면을 띄우기 위해 home.jsp 를 열어서 간단한 코드를 작성하고 서버를 실행하여 확인합니다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ page session="false" %>
<html>
<head>
	<title>Home</title>
</head>
<body>
<h1>
	Hello world!  
</h1>

<P> <a href="/home/ajaxView">Ajax로 이동하기</a> </P>
</body>
</html>

```



Src/main/java>com.cali.home 에 HomeController.java 를 열어서 코드 정리를 해줍니다.

```java
package com.cali.home;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class HomeController {
	
	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String home() {
		return "home";
	}
	
}
```



Src/main/java>com.cali.home 에 AjaxController.java 를 만들고 `@Controller` 를 만들어서 매핑을 합니다. 

```java
package com.cali.home;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class AjaxController {
	@RequestMapping("/ajaxView")
	public String ajaxHome() {
		return "ajax/ajaxView";
	}
}

```



ajaxView.jsp 로 와서 찍어줄수 있도록 작성합니다. 

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>비동기식으로 controller 에 접속하여 정보를 리턴받는다.</h1>
<hr/>
<div id="view" style="border:1px solid gray;"></div>
</body>
</html>
```



작성 후 서버를 재시작하여 확인해봅니다. 



이번에는 비동기식으로 서버에 접속하여 문자열을 결과로 리턴받는 작업을 해봅시다. 먼저 ajaxView.jsp (클라이언트 페이지) 에가서 버튼을 하나 추가합니다.

```jsp
<button id="ajaxString">ajax 문자열</button>
```



 jQuery 로 실행을 할것이기 때문에 CDN 을 가져오고 비동기식으로 서버에 접속하여 문자열을 결과로 리턴받는 코드를 작성합니다.

```jsp
<!-- ajaxView.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script>
	$(function(){
		$("#ajaxString").click(function(){
			// 비동기식으로 서버에 접속하여 문자열을 결과로 리턴받는다.
			// ajax
			var url = "/home/ajaxString";
			var params = "num=123&name=john&tel=650-999-9999";
			$.ajax({ // 클릭하면 서버에 접속해서 결과를 가져온다.
				url : url,
				data : params,
				type : "GET",
				success : function(result){
					$("#view").append(result);
				},
				error: function(e){
					console.log(e.responseText);
				}
			});

		});
	});
</script>
</head>
<body>
<h1>비동기식으로 controller 에 접속하여 정보를 리턴받는다.</h1>
<button id="ajaxString">ajax 문자열</button>
<hr/>
<div id="view" style="border:1px solid gray;"></div>
</body>
</html>
```



ajax가 접속할 매핑주소를 AjaxController.java (서버 페이지)에 작성합니다. 

```java
<!-- AjaxController.java -->
package com.cali.home;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class AjaxController {
	@RequestMapping("/ajaxView")
	public String ajaxHome() {
		return "ajax/ajaxView";
	}
	
	@RequestMapping(value="/ajaxString", produces="application/text; charset=UTF-8") // 접속을 받아낼 매핑주소
	@ResponseBody // 리턴되는 값이 뷰가 아닌 데이터이다. 
	public String ajaxString(int num, String name, String tel) {
		System.out.println(num+", "+name+", "+tel);
		
		String data = "번호:"+num+"<br/>이름:"+name+"<br/>연락처:"+tel+"</p>";
		
		return data;
	}
}
```





## 객체 받아오기 

다음은 VO 객체를 받아오는 방법을 구현해 봅시다.

src/main/java>com.cali.hom 에 TestVO.java 를 생성하고 getter/setter 를 만듭니다.

```java
<!-- TestVO.java -->
package com.cali.home;

public class TestVO {
	private int no;
	private String username;
	private String tel;
	private String addr;
	
	public int getNo() {
		return no;
	}
	public void setNo(int no) {
		this.no = no;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getTel() {
		return tel;
	}
	public void setTel(String tel) {
		this.tel = tel;
	}
	public String getAddr() {
		return addr;
	}
	public void setAddr(String addr) {
		this.addr = addr;
	}
	
}
```



다시 ajaxView.jsp 로 돌아와 button 을 하나 더 만듭니다. 

```jsp
<!-- ajaxView.jsp -->
<button id="ajaxObject">ajax 객체</button>
```

```javascript
<!-- ajaxView.jsp -->
// 객체 받아오기
		$("#ajaxObject").click(function(){
			$.ajax({
				url : "/home/ajaxObject",
				data : "num=568&username=MJ",
				type : "GET",
				success : function(obj){
					var tag = "<ol>";
					tag += "<li>번호 = "+obj.no+"</li>";
					tag += "<li>이름 = "+obj.username+"</li>";
					tag += "<li>연락처 = "+obj.tel+"</li>";
					tag += "<li>주소 = "+obj.addr+"</li></ol>";
					// 결과를 view 에 출력하기
					$("#view").append(tag);
				},
				error : function(){
					console.log("에러발생")
				}
			});
		});
```



AjaxController.java 로 돌아와서 접속을 받아낼 매핑주소를 작성합니다. 

```java
<!-- AjaxController.java -->
  @RequestMapping("/ajaxObject")
	@ResponseBody
	public TestVO ajaxObject(int num, String username) {
		System.out.println(num+", "+username);
		
		TestVO vo = new TestVO();
		vo.setNo(9999);
		vo.setUsername("Jono");
		vo.setTel("650-000-0000");
		vo.setAddr("570 21st Street, Oakland");
		
		return vo;
	}
```



서버를 재실행하여 확인해봅니다. 버튼을 클릭하면 서버에서 객체를 받아옵니다.



## List 받아오기

이번에는 서버에서 ajax 리스트를 리턴받는 방법을 구현해보겠습니다.

다시 ajaxView.jsp 로 돌아와 button 을 하나 더 만듭니다. 

```jsp
<!-- ajaxView.jsp -->
<button id="ajaxList">ajax 리스트</button>
```

TestVO.java 에 객체를 담을 수 있는 코드를 작성합니다.

```java
<!-- TestVO.java -->
	public TestVO() {}
	public TestVO(int no, String username, String tel, String addr) {
		this.no = no;
		this.username = username;
		this.tel = tel;
		this.addr = addr;
	}
	
```



AjaxController.java 로 돌아와서 접속을 받아낼 매핑주소와 더미 데이터셋을 작성합니다. 

```java
<!-- AjaxController.java -->
  @RequestMapping("/ajaxList")
	@ResponseBody // 리턴되는 값이 뷰가 아닌 데이터이다.
	public List<TestVO> ajaxList(){
		List<TestVO> lst = new ArrayList<TestVO>();
		
		lst.add( new TestVO(1, "John", "605-000-9999", "570 21st Street, Oakland"));
		lst.add( new TestVO(1, "MJ", "605-111-1111", "570 22st Street, Oakland"));
		lst.add( new TestVO(1, "Andrew", "605-222-2222", "570 23st Street, Oakland"));
		lst.add( new TestVO(1, "Kaycee", "605-333-3333", "570 24st Street, Oakland"));
		
		return lst;
	}

```



비동기식으로 서버에 접속하여 List 객체를 결과로 리턴받는 코드를 작성합니다.

```javascript
<!-- ajaxView.jsp -->
		// List객체 받아오기
		$("#ajaxList").click(function(){
			$.ajax({
				url : "/home/ajaxList",
				success : function(result){
					var tag = "<ul>";
					
					// 서버에서 전달받은 컬렉션 리스트를 반복문을 돌리기 위해서는
					// 컬렉션이 담겨져있는 변수를 선택자로 지정해야한다.
					var $result = $(result);
					
					$result.each(function(idx, vo){
						tag += "<li>"+ vo.no+ ", "+vo.username+", "+vo.tel+", "+vo.addr+"</li>";
					});
					
					tag += "</ul>";
					$("#view").append(tag);
					
				}, error : function(){
					console.log("List객체 에러발생")
				}
			});
		});
```



서버를 재실행하여 확인해봅니다. 버튼을 클릭하면 화면이 바뀌지 않고 서버에서 리스트 객체를 받아옵니다.



## Map 가져오기



버튼을 추가하고 서버에 접속하는 ajax 기본 코드를 작성합니다.

```jsp
<!-- ajaxView.jsp -->
<button id="ajaxMap">ajax Map</button>
```

```javascript
<!-- ajaxView.jsp -->
		// Map 가져오기
		$("#ajaxMap").click(function(){
			$.ajax({
				url:"/home/ajaxMap",
				success:function(result){
					
				},error:function(){
					console.log("에러발생");
				}
			});
		});
```



서버에 가서 Map 을 만들어 보겠습니다.

```java
<!-- AjaxController.java -->

	@RequestMapping("ajaxMap")
	@ResponseBody
	public HashMap<String, TestVO> ajaxMap() {
		HashMap<String, TestVO> map = new HashMap<String, TestVO>();
		
		map.put("m1", new TestVO(100, "Eminem", "650-111-1111", "Oakland"));
		map.put("m2", new TestVO(200, "Drake", "650-222-2222", "New York"));
		map.put("m3", new TestVO(300, "Snoop", "650-333-3333", "LA"));
		
		return map;
	}
```



클라이언트 페이지로 돌아와 출력 코드를 작성합니다.

```javascript
<!-- ajaxView.jsp -->
		// Map 가져오기
		$("#ajaxMap").click(function(){
			$.ajax({
				url:"/home/ajaxMap",
				success:function(result){
					var tag = "<ul>";
					tag += "<li>"+result.m1.no+", "+result.m1.username+", "+result.m1.tel+", "+result.m1.addr+"</li>";
					tag += "<li>"+result.m2.no+", "+result.m2.username+", "+result.m2.tel+", "+result.m2.addr+"</li>";
					tag += "<li>"+result.m3.no+", "+result.m3.username+", "+result.m3.tel+", "+result.m3.addr+"</li>";
					tag += "</ul>";
					$("#view").append(tag);
				},error:function(){
					console.log("에러발생");
				}
			});
		});
```





## JSON 데이터 가져오기

이번에는 ajax 를 이용하여 서버로 JSON 데이터를 보내봅시다.

버튼을 생성하고 기본 ajax 코드를 작성합니다.

```jsp
<button id="ajaxJson">ajax Json</button>
```

```javascript
		// ajax 를 이용하여 서버로 json 데이터보내기
		//			클라이언트 json 형식을 문자열 보내기
		$("#ajaxJson").click(function(){
			var url="/home/ajaxJson";
			var jsonData = {
					num : 100,
					name : "John",
					tel : "650-999-9999"
			}
			$.ajax({
				url:url,
				type:"GET",
				data:jsonData,
				dataType:"json",
				success:function(result){
					
				},
				error:function(e){
					console.log(e.responseText);
				}
			});
		});
```



서버에 가서 JSON 을 만들어보겠습니다.

```java
	@RequestMapping("/ajaxJson")
	@ResponseBody
	public String ajaxJson(int num, String name, String tel) {
		
		// 클라이언트측에서 서버로 전송된 Json 데이터
		System.out.printf("%d, %s, %s\n", num, name, tel);
		
		int code=5865;
		String productName = "컴퓨터";
		int price = 15000;
		
		//		{"code":"5865", "productName":"컴퓨터", "price":"15000"}
		String jsonData = "{\"code\":\""+code+"\"";
		jsonData += ",\"productName\":\""+productName+"\"";
		jsonData += ",\"price\":\""+price+"\"}";
		
		System.out.println(jsonData);
		return jsonData;
		
	}
```



다시 클라이언트 페이지로 돌아와서 출력 코드를 작성합니다.

```javascript
		// ajax 를 이용하여 서버로 json 데이터보내기
		//			클라이언트 json 형식을 문자열 보내기
		$("#ajaxJson").click(function(){
			var url="/home/ajaxJson";
			var jsonData = {
					num : 100,
					name : "John",
					tel : "650-999-9999"
			}
			$.ajax({
				url:url,
				type:"GET",
				data:jsonData,
				dataType:"json",
				success:function(result){
					// json 데이터로 받아짐.
					console.log(result);
					// 문자열을 json 으로 변환하여 확인해보기
					var jsonString = JSON.stringify(result); // json 을 문자화
					console.log(jsonString);
					var json = JSON.parse(jsonString); // 문자를 json 으로
					console.log(json);
					
					var tag = "<ul>";
					tag += "<li>코드 : "+ result.code+"</li>";
					tag += "<li>상품명 : "+ result.productName+"</li>";
					tag += "<li>가격 : "+ result.price+"</li></ul>";
					$("#view").append(tag);
				},
				error:function(e){
					console.log(e.responseText);
				}
			});
		});
```

