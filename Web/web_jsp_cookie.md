# JSP: Cookie

> Wed Jul 20, 2022

---

[toc]



Cookie 란?

클라이언트 컴퓨터에 기록되어 관리되는 데이터입니다.



백엔드에서 쿠키를 저장합니다.

JSP 의 경우 서버에서 실행하여 클라이언트로 보여지기 때문에 response 를 사용합니다.

Cookie 클래스를 객체로 만들어 response 를 이용하여 클라이언트 컴퓨터에 정보를 보낸다. 



### JSP 에서 쿠키를 생성하는 방법

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<%
	// 백엔드(jsp) 쿠키저장
	// Cookie 클래스를 객체로 만들어 response 를 이용하여 클라이언트 컴퓨터에 정보를 보낸다. 
	//						    변수, 값
	Cookie cookie = new Cookie("userid", "goguma");
	Cookie cookie2 = new Cookie("nickname", "고구마");
	
	// 소멸시점설정
	cookie.setMaxAge(60*3); // 쿠키의 생명주기를 초단위로 설정합니다.
	
	// 클라이언트에게 전송
	response.addCookie(cookie);
	response.addCookie(cookie2);
%>
</head>
<body>
<h1><a href="">쿠키확인하기</a></h1>

</body>
</html>
```



### cookieView

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
<ol>
	<%
		// 클라이언트 컴퓨터의 쿠키정보를 서버로 가져오기
		
		Cookie[] coo = request.getCookies();
	
		for(int idx=0; idx<coo.length; idx++){
	%>
			<li><%=coo[idx].getName() %> : <%=coo[idx].getValue() %></li>
		
	<%  }%>
</ol>
</body>
</html>
```

