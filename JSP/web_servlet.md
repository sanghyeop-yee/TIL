# Servlet

> Fri Jul 22, 2022

---

[toc]

webServlet 프로젝트를 새로 만듭니다.

Servlet 를 이용하여 로그인 페이지를 만들어봅시다.

MySQL 데이터베이스와 연동을 하기때문에 WEB-INF>lib 폴더에 mysql-connector-java-8.0.29.jar 파일을 붙여넣습니다.



### web.xml

```xml
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

  <servlet>
	  <servlet-name>loginOk</servlet-name>
	  <servlet-class>com.mulcam.webapp.ServletTest</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>loginOk</servlet-name>
  	<url-pattern>/loginOk.do</url-pattern>
  </servlet-mapping>
  
  <servlet>
  	<servlet-name>index</servlet-name>
  	<servlet-class>com.mulcam.webapp.ServletIndex</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>index</servlet-name>
  	<url-pattern>/index.do</url-pattern>
  </servlet-mapping>
  
  <servlet>
  	<servlet-name>logout</servlet-name>
  	<servlet-class>com.mulcam.webapp.ServletLogout</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>logout</servlet-name>
  	<url-pattern>/logout.do</url-pattern>
  </servlet-mapping>
</web-app>


```





### 로그인 폼 : ServletTest.java

```java
package com.mulcam.webapp;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;


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
		pw.print("<h1>로그인</h1><form method='post' action='/webServlet/loginOk.do'/>");
		pw.print("Id : <input type='text' name='userid'/><br/>");
		pw.print("Password : <input type='password' name='userpwd'/><br/>");
		pw.print("<input type='submit' value='Login'/></form></body></html>");
		pw.close();
		
	}


	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// 로그인
		String userid = request.getParameter("userid");
		String userpwd = request.getParameter("userpwd");
		
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		
		// DB에 아이디와 비밀번호가 일치할 경우 이름을 선택한다.
		try {
			// 1. 드라이브 로딩
			Class.forName("com.mysql.cj.jdbc.Driver");
			
			// 2. DB 연결
			// db서버주소, 아이디, 비밀번호
			conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1/mydb", "root", "djftlrn9");
			
			// 3. Statement 생성
			String sql = "select username from member where userid=? and userpwd=?";
			pstmt = conn.prepareStatement(sql);
			pstmt.setString(1,  userid);
			pstmt.setString(2, userpwd);
			
			// 4. 실행
			rs = pstmt.executeQuery();
			
			response.setContentType("text/html; charset=UTF-8");
			PrintWriter pw = response.getWriter();
			if(rs.next()) { // 선택한 레코드가 있을때 == 로그인 성공
				// request 에서 session 객체를 구하여 로그인 여부를 등록한다.
				HttpSession session = request.getSession();
				session.setAttribute("logId", userid);
				session.setAttribute("logName", rs.getString(1));
				
				// 웹페이지로 이동 "홈으로" 
				pw.println("<script>");
				pw.println("alert('로그인 성공하였습니다. 홈페이지로 이동합니다.');");
				pw.println("location.href='/webServlet/index.do';");
				pw.println("</script>");
				
			}else { // 로그인 실패
				// 웹페이지로 이동 "다시 로그인 페이지로"
				pw.println("<script>");
				pw.println("alert('로그인 실패하였습니다. 로그인 페이지로 이동합니다.');");
				pw.println("history.back();");
				pw.println("</script>");
				
			}
			pw.close();
			
		}catch(Exception e) {
			e.printStackTrace();
		}finally {
			// 5. 닫기
			try {
				if(rs!=null) rs.close();
				if(pstmt!=null) pstmt.close();
				if(conn!=null) conn.close();
			}catch(Exception ee) {}
			
		}
	}

}

```





### ServletIndex.java

```java
package com.mulcam.webapp;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;


@WebServlet("/ServletIndex")
public class ServletIndex extends HttpServlet {
	
	
    public ServletIndex() {
        super();

    }
    

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// 홈페이지 구성
		HttpSession session = request.getSession();
		String userid = (String)session.getAttribute("logId");
		String username = (String)session.getAttribute("logName");
		
		response.setContentType("text/html; charset=UTF-8");
		
		PrintWriter pw = response.getWriter();
		
		pw.println("<html>");
		pw.println("<head><title>홈페이지</title></head>");
		pw.println("<body>");
		pw.println("<h1>서블릿 홈페이지</h1>");
		
		if(userid==null) {
			pw.println("<a href='/webServlet/main.do'>로그인</a>");
			pw.close();
		}else { // 로그아웃
			// 
			pw.println(username+"님<a href='/webServlet/logout.do'>로그아웃</a>");
		}
		pw.close();
	}
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}

```





### ServletLogout.java

```java
package com.mulcam.webapp;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet("/ServletLogout")
public class ServletLogout extends HttpServlet {
	private static final long serialVersionUID = 1L;
       

    public ServletLogout() {
        super();

    }


	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// 로그아웃 구현
		request.getSession().invalidate();
		
		// 홈으로 이동
		response.sendRedirect("/webServlet/index.do");
	}


	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		doGet(request, response);
	}

}

```



