# 로그인





view 페이지는 jsp 로 spring 으로 백앤드를 작업하여 프로젝트를 만들어 봅시다. DB 작업은 Mybatis 를 이용합니다. 필요한 프레임워크들을 메이븐저장소에서 가져옵니다. 



Pom.xml 을 열어서 자바와 스프링 버전을 셋팅해줍니다.

```xml
	<properties>
		<java-version>11</java-version>
		<org.springframework-version>5.2.22.RELEASE</org.springframework-version>
		<org.aspectj-version>1.6.10</org.aspectj-version>
		<org.slf4j-version>1.6.6</org.slf4j-version>
	</properties>
```



pom.xml 의 <dependencies> 가장 밑에 아래 코드를 추가해줍니다.



Maven Repository > Spring JDBC > 5.2.22.RELEASE

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.22.RELEASE</version>
</dependency>

```



Maven Repository > MySQL Connector/J > 8.0.29

```xml
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.29</version>
</dependency>

```



Maven Repository > MyBatis Spring > 2.0.7

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.7</version>
</dependency>

```



Maven Repository > MyBatis > 3.5.10

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.10</version>
</dependency>

```



src/main/java 에 controller, dao, service, vo 패키지들을 생성해줍니다.

src/main/resources 에 mapper 폴더를 생성해주고 mybatis-config.xml 파일을 만들어줍니다.



root-context.xml 에 다음과 같이 작성합니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<!-- ioc 컨테이너 -->
	<!-- Root Context: defines shared resources visible to all other web components -->
	<!-- MySQL 서버 설정 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
		<property name="url" value="jdbc:mysql://127.0.0.1/california"></property>
		<property name="username" value="root"></property>
		<property name="password" value="djftlrn9"></property>
	</bean>
	<bean id="SqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource"/>
		<property name="configLocation" value="classpath:mybatis-config.xml"></property>
		<property name="mapperLocations" value="classpath:mapper/*Mapper.xml"></property>
	</bean>
	
	<mybatis-spring:scan base-package="com.california.myapp.dao"/>
	
</beans>

```



작성 후 web.xml 에 아래 코드를 추가합니다.

```xml
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml, /WEB-INF/spring/root-context.xml</param-value>
		</init-param>
```



com.california.myapp.vo 패키지 안에 MemberVO 클래스를 생성하고 getter setter 를 생성합니다.



MemberDAO, MemberService interface 를 생성하고 MemberServiceImp 클래스를 생성합니다.



MemberMapper.xml 을 생성합니다.





---

webapp> Inc 폴더 생성 후 jspf 파일을 생성합니다. Servlet-context.xml 에 inc 폴더의 위치를 추가해줍니다.

```xml
	<!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
	<resources mapping="/resources/**" location="/resources/" />
	<resources mapping="/inc/**" location="/inc/" />
```



#### top.jspf



#### bottom.jspf



#### style.css



web.xml 에 include 로 추가해줍니다.

```xml
	<!-- include -->
	<jsp-config>
		<jsp-property-group>
			<url-pattern>*.jsp</url-pattern>
			<include-prelude>/inc/top.jspf</include-prelude>
			<include-coda>/inc/bottom.jspf</include-coda>
		</jsp-property-group>
	</jsp-config>
```







memberController.java 에 추가합니다.



유효성 검사까지 마쳤으니 이제 DB 작업을 실행합니다.

