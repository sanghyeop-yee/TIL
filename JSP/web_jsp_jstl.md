# JSTL

> Thu Jul 21, 2022

---

[toc]

> JSTL : JSP Standard Tag Library
>
> taglib 지시어는 JSP 페이지에 사용할 커스텀 태그 라이브러리를 지정할 때 사용합니다.



https://tomcat.apache.org/taglibs/standard 으로 접속하여 이미 compile 이 된 폴더인 bin 으로 들어가서 jakarta-taglibs-standard-1.1.2.zip  을 다운로드 받습니다. 

참조를 하기 위해서는 이클립스의 WEB-INF/lib 폴더안으로 jstl.jar 과 standard.jar 를 복사해놓습니다.  



webJSP 프로젝트내의 모든 JSP 파일에 tag library 를 사용하기 위해 템플릿에 아래 코드를 추가해줍니다.

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```



## set / remove 태그 : 변수선언 및 삭제

### index.jsp

다음 코드를 추가하여 링크를 걸어봅시다.

````jsp
	<h2>jstl(jsp standard tag library)</h2>
	<div>
		<p style="background-color: #ddd">
		https://tomcat.apache.org/taglibs/standard 에서 다운로드
		jarkarta-taglibs-standard-1.1.2.zip 다운로드
		이클립스의 WEB-INF/lib 폴더에 jstl.jar, standard.jar 를 복사한다.
		</p>
		<ol>
			<li><a href="/webJSP/jsp07_jstl/setTag.jsp">set Tag : 변수를 선언하고 삭제하는 방법</a>
			<li><a href="/webJSP/jsp07_jstl/ifTag.jsp?name=John&age=500&tel=650-805-4900">if Tag : 조건문</a>
			<li><a href="/webJSP/jsp07_jstl/forEach.jsp">forEach Tag : 반복문</a>
			<li><a href="/webJSP/jsp07_jstl/url.jsp">url Tag</a></li>
			<li><a href="/webJSP/jsp07_jstl/choose.jsp?name=hong&age=25">choose Tag : 조건문</a></li>
      <li><a href="/webJSP/jsp07_jstl/redirect.jsp">redirect Tag : 자동페이지 이동</a></li>
		</ol>
	</div>
````



### setTag.jsp

```jsp
<%@ page import="java.util.Date" %>
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!--  tag library 를 사용하기 위해서는 지시부에 태그라이브러리 정보를 표시하여햐 한다. -->
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>set태그 : 변수선언 및 삭제</h1>
<%
	int a = 123;
	String b = "Rain";
	
%>
<c:set var="num" value="<%=a %>"/>
<c:set var="no" value="${567 }"/>
<c:set var="name">세종대왕</c:set>
<c:set var="now" value="<%=new Date() %>"/>

<h2>변수선언</h2>
a = ${c+10}<br/>
num = ${num }<br/>
no = ${no }<br/>
name = ${name }<br/>
now = ${now }

<h2>변수삭제</h2>
<c:remove var="no"/>
no : ${no }
</body>
</html>
```





## if 태그 : 조건문

### ifTag.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

<h1>if 태그 : 조건문</h1>
<c:set var="n" value="${10}"/>
<c:set var="x" value="${5}"/>

<!--  >, <, >=, <=, !=, ==  -->
<c:if test="${n>x}">
	참일때 실행된다....
</c:if>

<c:if test="${n<x }">
	참일때 실행된다~~~~
</c:if>

<c:if test="${true }">
	무조건 참이다...
	<img src="../img/sf04.jpeg" width='100'/>
</c:if>

<h1>jstl에서 request하기, request.getParameter("name") -> param.name</h1>
<c:set var="name" value="${param.name }"/>
Name : ${name}<br/>
Age : ${param.age}<br/>
Tel : ${param.tel}<br/>
```





## forEach 태그 : 반복문

### forEach.jsp

```jsp
<%@page import="java.util.HashMap"%>
<%@page import="java.util.ArrayList"%>
<%@page import="java.util.List"%>
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

<h1>forEach태그 : 반복문</h1>


<h2>숫자를 이용한 반복문</h2>
<!-- 			 시작    종료     증가  -->
<c:set var="dan" value="7"/>
<c:forEach var="n" begin="2" end="9" step="1">
	${dan } * ${n } = ${dan*n }<br/>
</c:forEach>


<h2>배열을 이용한 반복문</h2>
<%
	int arr[] = {43,65,876,32,65,76,32,76,23};
%>
<c:forEach var="data" items="<%=arr %>">
	[${data }],
</c:forEach>


<h2>컬렉션 : ArrayList를 이용해서 반복문 (제일많이 사용됨)</h2>
<%
	List<String> lst = new ArrayList<String>();
	lst.add("John");
	lst.add("MJ");
	lst.add("Jono");
	lst.add("Andrew");
	lst.add("Kaycee");
	lst.add("Erik");
%>
<c:forEach var="name" items="<%=lst %>">
	[${name }]

</c:forEach>
	

<h2>Map을 이용한 반복문</h2>
<% 
	HashMap<String, String> hm = new HashMap<String, String>();

	hm.put("name", "John");
	hm.put("addr", "570 21st Street, Oakland");
	hm.put("tel", "650-999-9999");
	hm.put("email", "myemail@gmail.com");
%>
<c:forEach var="mapData" items="<%=hm %>">
	Key : ${mapData.key }, Value : ${mapData.value }<br/>	
</c:forEach> 

<h1>forTokens 태그 : 문자열을 특정 문자로 조각내기</h1>
<c:forTokens var="t" items="red,blue,yellow.purple.orange-brown^^green-indigo" delims=",-">
	[[${t }]]
</c:forTokens>
```





## url 태그 : url 주소를 가지는 태그

### url.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<h1>url 태그 : url 태그를 가지는 태그</h1>
<c:url var="home" value="/index.jsp"/>
<c:url var="login" value="/jsp02_response/login.jsp"/>
	<c:param name="userid" value="goguma777"></c:param>
	<c:param name="userpwd" value="1234"/>

<a href="/webJSP/index.jsp">Home</a>
<a href="/webJSP/jsp02_response/login.jsp">Login</a><br/>

<a href="${home }">홈</a>
<a href="${login }">로그인</a>

</body>
</html>
```



### login.jsp

로그인 폼에서 value 로 전달받은 값을 사용할 수 있습니다.

```jsp
<h1>로그인</h1>
<form method="post" action="loginOk.jsp">
	Id : <input type="text" name="userid" id="userid" value="${param.userid }"/><br/>
	Password : <input type="password" name="userpwd" id="userpwd" value="${param.userpwd }"/><br/>
	<input type="submit" value="로그인"/>
</form>
```





## Choose 태그 : 조건문

### choose.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

<c:set var="name" value="${param.name}"/>
<c:set var="age" value="${param.age }"/>

<c:choose>
	<c:when test="${name=='hong'}">
		당신의 이름은 ${name }입니다.
	</c:when>
	<c:when test="${age>20 }">
		당신은 20세 이상입니다.
	</c:when>
	<c:otherwise>
		당신의 이름은 hong 도 아니고 나이가 20세 이상도 아닙니다.
	</c:otherwise>
</c:choose>
```



## Redirect 태그 : 페이지 자동이동

### login.jsp

로그인 폼에서 value 로 전달받은 값을 사용할 수 있습니다.

```jsp
<body>
<h1>로그인</h1>
<form method="post" action="loginOk.jsp">
	Id : <input type="text" name="userid" id="userid" value="${param.userid }"/><br/>
	Password : <input type="password" name="userpwd" id="userpwd" value="${param.userpwd }"/><br/>
	<input type="submit" value="로그인"/>
</form>
<hr/>
	Name : ${param.username }</br>
	Tel : ${param.tel }
</body>
```



### redirect.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

<c:redirect url="/jsp02_response/login.jsp">
	<c:param name="username">John</c:param>
	<c:param name="tel" value="504-599-4949"/>
</c:redirect>
```

