# Springboot

> Fri Jul 29, 2022

---





Spring 은 메이븐 방식으로 pom.xml 에 작성하면 자동으로 다운로드가 되고 tomcat 을 서버로 사용합니다.

Springboot 은 maven 방식과 gradle 을 사용하고 내장된 서버를 사용합니다.

Mybatis (xml) > DAO > Controller > Service > VO > Views (.jsp) 의 구조는 동일합니다.

단지 차이는 서버를 maven, gradle 로 할 것인지만 정하면 됩니다. 



SpringBoot 를 다운받고 Create new Spring Starter Project 를 생성합니다. 

Project Name: bootMybatisMysql
Type: Gradle Project 
Packaging: War
Java Version: 11
Group: com.cali
Package: com.cali.myapp

다음으로 넘어가서 아래를 선택해줍니다.

New Spring Starter Project Dependencies

* Developer Tools: 
  * Spring Boot DevTools, Lombok
* SQL:
  * JDBC API
  * MyBatis Framework
  * MySQL Driver
* Web:
  * Spring Web



src>main 에 webapp > WEB-INF > views 폴더를 새로 만듭니다.

뷰를 jsp로 사용하기 위해서 프레임워크를 추가해야합니다. 메이븐저장소 > tomcat embed jasper > 9.0.58 > Gradle (Short) > 복사하여 build.gradle 을 열어 붙여넣습니다. 

추가로 JSTL, Inject (자동으로 객체 만들어줌) 을 검색해서 추가합니다.

```xml
	// 뷰를 jsp로 사용하기 위해서 프레임워크를 추가한다.
	// https://mvnrepository.com/artifact/org.apache.tomcat.embed/tomcat-embed-jasper
	implementation 'org.apache.tomcat.embed:tomcat-embed-jasper:9.0.58'

	// https://mvnrepository.com/artifact/javax.servlet.jsp.jstl/jstl
	implementation 'javax.servlet.jsp.jstl:jstl:1.2'

// https://mvnrepository.com/artifact/javax.inject/javax.inject
implementation group: 'javax.inject', name: 'javax.inject', version: '1'

```



pom.xml 을 저장만해도 자동으로 다운이 되지만 build.gradle 을 마우스 오른쪽을 눌러서 Gradle > Refresh Gradle Project 를 눌러줘야 다운이 됩니다.