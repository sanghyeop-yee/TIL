# JSP: Include

> Wed Jul 20, 2022

---

[toc]

프로젝트내에 모든 파일에 같은 header 나 footer 를 적용하고 싶다면 include 를 활용할 수 있습니다. include 를 하는 경우 중복되는 코드가 있을수 있기때문에 소스정리를 해주어야 합니다.

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



## JSPF

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







### web.xml

webApp 프로젝트 전체에 활용하고 싶다면 Web.xml 파일을 WEB-INF 폴더 안으로 복사하고 <jsp-config> 부분을 붙여넣습니다.

```jsp
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
  version="4.0"
  metadata-complete="true">

  <display-name>Welcome to Tomcat</display-name>
  <description>
     Welcome to Tomcat
  </description>
  <jsp-config>
  	<jsp-property-group>
  		<url-pattern>*.jsp</url-pattern>
  		<include-prelude>/jsp06_include/topMenu.jspf</include-prelude>
  		<include-coda>/jsp06_include/bottomInc.jspf</include-coda>
  	</jsp-property-group>
  </jsp-config>
</web-app>

```



### topMenu.jspf

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style>
	#menu{
		height:100px; background-color:lightgreen;
	}
</style>
</head>
<body>
<div id="menu"></div>

```



### sessionSave.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<h1>세션에 데이터 기록하기</h1>
<a href="/webJSP/index.jsp">홈</a>

<%
	session.setAttribute("logId", "goguma");
	session.setAttribute("logStatus", "Y");
	session.setAttribute("logName", "고구마");
%>

```





### bottomInc.jspf

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<style>
	#bottomInc{
		height:50px; background-color:blue;
	}
</style>
<div id="bottomInc"></div>

</body>
</html>
```



