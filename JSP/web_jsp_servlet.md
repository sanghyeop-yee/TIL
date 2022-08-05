# Servlet

> Thu Jul 21, 2022

---

[toc]

### Servlet 이란?

JSP 로 웹사이트를 만들때 Mode 1, Mode 2 (MVC 패턴) 두가지 방식이 있습니다.

JSP 에서 모든것이 이루어지는 Mode 1 는 비교적 쉽지만 debugging 이 어렵기 때문에 요새는 잘 안쓰입니다.

회원정보 폴더 안에 (회원가입, 회원정보수정, 회원탈퇴, 로그인, 로그아웃, 아이디찾기, 비번찾기, 비번변경) 파일을 각각 만들어야 하죠. 

게시판의 경우도 마찬가지입니다. (글등록, 글보기, 글수정, 글삭제, 댓글(등록, 수정, 삭제))



요새는 view 페이지는 JSP 로 서버는 java 로 주로 만듭니다.



### MVC 패턴이란?

MVC (Model - View - Control) 하는 일의 역할을 나눠서 작업하는것을 말합니다. 나중에 Spring 으로 가서 만들겁니다.



Servlet 을 통해서 웹페이지를 어떻게 구성하는지 배워봅시다.

webServlet 프로젝트를 새로 만듭니다.

반드시 패키지를 3개를 만들어 합니다.

com.calif.webapp 으로 패키지를 만듭니다. 하위에는 패키지 또는 클래스가 생성될 수 있습니다.



### ServletTest.java

com.calif.webapp 패키지 하위에 Servlet 클래스를 하나 만듭니다. 

```java
package com.mulcam.webapp;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet("/ServletTest")
public class ServletTest extends HttpServlet {
	private static final long serialVersionUID = 1L;
       

    public ServletTest() {
        super();
       
    }


	protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
		System.out.println("doGet()메소드 실행됨");
		
		// web.xml 의 값을 Servlet 클래스로 가져오기
		String userid = getInitParameter("userid");
		System.out.println("userid->"+userid);
		
		res.setContentType("text/html; charset=UTF-8");
		
		PrintWriter pw = res.getWriter();
		
		pw.print("<html>");
		pw.print("<head>");
		pw.print("<title>로그인 폼</title></head><body>");
		pw.print("<h1>로그인</h1><form method='post' action=''/>");
		pw.print("Id : <input type='text' name='userid'/><br/>");
		pw.print("Password : <input type='password' name='userpwd'/><br/>");
		pw.print("<input type='submit' value='Login'/></form></body></html>");
		pw.close();
		
	}


	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("doPost()메소드 실행됨");
	}

}

```





### web.xml

다음 tomcat dml ROOT > WEB-INF 에서 web.xml 를 복사하여 이클립스 프로젝트의 WEB-INF 폴더에 붙여넣습니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
 Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
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
  
  <servlet>
  	<servlet-name>home</servlet-name>
  	<servlet-class>com.mulcam.webapp.ServletTest</servlet-class>
  	<init-param>
  		<param-name>userid</param-name>
  		<param-value>John</param-value>
  	</init-param>
  </servlet>
  <servlet-mapping>
  	<servlet-name>home</servlet-name>
  	<url-pattern>/main.do</url-pattern>
  </servlet-mapping>

</web-app>

```



`http://localhost:8080/webServlet/main.do` 로 접속하여 확인합니다.



