# JSP

> Tue Jul 19, 2022

---

[toc]

JSP 는 웹에서 무슨 역할을 할까요?

> JSP 란 JavaServer Pages 의 약자이며 HTML 코드에 JAVA 코드를 넣어 동적웹페이지를 생성하는 웹어플리케이션 도구입니다. JSP 가 실행되면 자바 서블릿(Servlet) 으로 변환되며 웹 어플리케이션 서버에서 동작되면서 필요한 기능을 수행하고 그렇게 생성된 데이터를 웹페이지와 함께 클라이언트로 응답한다.



실습을 통해 배워봅시다. 우선 새로운 프로젝트를 만듭니다.

이후에 webApp 에 index.jsp 를 만듭니다.



Calendar 같은 패키지를 가져오기 위해서는 맨위의 지시부에 표시합니다.

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ page import="java.util.Calendar, java.io.FileReader"%>
<%@ page import="java.util.Scanner" %>
```



클라이언트에서 웹서버를 불러오면 jsp 부분은 웹서버내에서 실행, html 은 클라이언트로 불러오게 됩니다. 클라이언트에 보여질 내용이 있다면 `out.print("c="+c);` 이런식으로 작성합니다.



```jsp
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
```



메소드를 만들어서 호출하고 싶다면 페이지 상단 선언부에 표시합니다.

```jsp
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

<%
	out.print(gugudan(7));
%>
```





## a태그를 이용하여 서버로 데이터 보내기

내장객체 request 를 사용하여 클라이언트 측의 데이터를 서버로 가져옵니다.

### aLink.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>a태그를 이용한 서버로 데이터 보내기</h1>
<a href="aLinkOk.jsp?num=125&username=John&tel=605-490-9909">a 태그는 GET방식으로 데이터가 전송된다.</a>
</body>
</html>

```



### aLinkOk.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%
	// request : 클라이언트측의 데이터를 서버로 가져오기
	int num = Integer.parseInt(request.getParameter("num"));
	String username = request.getParameter("username");
	String tel = request.getParameter("tel");
	
%>
<img src="../img/sf01.jpeg"/>
<hr/>
num : <%=num %><br/>
username : <%=username %><br/>
tel : <%=tel %>
```





## 폼을 이용하여 서버로 데이터 보내기

### form.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>폼을 이용한 서버로 데이터 보내기</h1>
<form method="post" action="formOk.jsp">
	Id : <input type="text" name="userid"/><br/>
	Password : <input type="password" name="userpwd"/><br/>
	Name : <input type="text" name="username"/><br/>
	
	<input type="submit" value="전송"/>
</form>
</body>
</html>

```



### formOk.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

<%
	// post 방식의 전송일때 한글을 인코딩해준다.
	request.setCharacterEncoding("UTF-8");

	String userid = request.getParameter("userid");
	String userpwd = request.getParameter("userpwd");
	String username = request.getParameter("username");
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
Id : <%=userid %><br/>
Password : <%=userpwd %><br/>
Name : <%=username %><br/>
<%
	System.out.println(username);
%>
</body>
</html>
```

