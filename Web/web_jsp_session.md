# JSP: Session

> Wed Jul 20, 2022

---

[toc]



Session 은 서버의 메모리에 기록이 됩니다.

세션에는 각자 할당되는 id 가 존재합니다. 설정된 시간이 지나면 세션안에 저장되어있는 데이터가 사라집니다. 



### sessionSave.jsp

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
<h1>세션에 데이터 기록하기</h1>
<a href="/webJSP/index.jsp">홈</a>

<%
	session.setAttribute("logId", "goguma");
	session.setAttribute("logStatus", "Y");
	session.setAttribute("logName", "고구마");
%>
</body>
</html>
```



### sessionLogout.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%
	// 로그아웃 세션에 있는 모든 기록을 제거한다.
	// 세션객체를 삭제하면 새로운 세션이 할당된다.
	
	// 세션 지우기
	session.invalidate(); 
	
	// 페이지이동
	response.sendRedirect("/webJSP/index.jsp");
%>
```



### index.jsp

```jsp
<!--  지시부  -->
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ page import="java.util.Calendar, java.io.FileReader"%>
<%@ page import="java.util.Scanner" %>
<%! // 선언부
	// 메소드나 변수를 선언하는 영역
	public String gugudan(int dan){
		String tag = "<ul>";
		for(int i=2; i<=9; i++){
			tag += "<li>" + dan + "*" + i + "=" + (dan*i) + "</li>";
		}
		tag += "</ul>";
		return tag;
	}
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style>
	header{height:100px; background:gold; color:white; line-height:100px; text-align:center; font-size:4em;}
</style>
</head>
<body>
<header>멀캠 홈페이지</header>
<% if(session.getAttribute("logStatus")!=null && session.getAttribute("logStatus").equals("Y")){ %>
	<%=session.getAttribute("logName") %><a href="/webJSP/jsp04_session/sessionLogout.jsp">로그아웃</a>
<% }else{ // 로그인 안됐을때 %>	
<!-- <a href="/webJSP">로그인</a> -->
	<a href="<%=request.getContextPath() %>/jsp02_response/login.jsp">로그인</a>
<% } %>


<h2><%=session.getId() %></h2>
<div>
<% 
	// 스크립트릿 : (자바) 명령어 입력하는 곳
	int a = 100;
	String name = "John";
	int c = a / 3;
	
	Calendar now = Calendar.getInstance();
	System.out.println("c="+c);
	
	// 내장 객체 : request, response, session, out, application, cookie
	
	out.print("c="+c);
%>
<hr/>
<%
	out.print("<h1>jsp에서 클라이언트에게 보낸 데이터</h1>");
	out.print(gugudan(7));
%>
<hr/>
a=<%=a+15%><br/>
name=<%=name %><br/>
c=<%=c %>

</div>
</body>
</html>
```

