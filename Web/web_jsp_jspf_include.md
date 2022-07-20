# JSP: Include

> Wed Jul 20, 2022

---

[toc]

### top.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%
	String name="John";
%>

<div id="top">
	<%=name %>
</div>

```

### bottom.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%
	int num = 1234;
%>

<div id="bottom">
	<%=num %>
</div>

```

### includeMain_jsp.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style>
	.container{
		width:80%; margin:0 auto;
	}
	.container>img{width:100%;}
	
	#top{height:50px; background-color:pink;}
	#bottom{height:50px; background-color:gray;}
</style>
</head>
<body>
<!-- top.jsp include -->
<jsp:include page="top.jsp"/>
<div class="container">
	<img src="../img/sf01.jpeg"/>
</div>
<!-- bottom.jsp include -->
<jsp:include page="bottom.jsp"></jsp:include>
</body>
</html>
```

### header.jspf

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%
	String username = "John";
%>
<style>
	#header{height:50px; background-color:lightgreen;}
</style>
<div id = "header">
<%=username %>
</div>
```

### footer.jspf

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<style>
	#footer{
		height:50px; background-color:orange;
	}
</style>
<div id="footer">
	<%=username %>
</div>

```

### includeMain_jspf.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style>
	#main{
		width:80%;
		margin:0 auto;
	}
	#main>img{
		width:100%;
	}
</style>
</head>
<body>
<%@ include file="header.jspf" %>
<div id="main">
	<%=username %><br/>
	<img src="../img/sf02.jpeg"/>
</div>
<%@ include file="footer.jspf" %>
</body>
</html>
```

