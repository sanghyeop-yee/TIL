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





----

이번에는 insert 문을 구현해봅니다.



---

Board 를 만들어봅시다.

Views > board 폴더 생성 후 com.cali.myapp 에 각각의 패키지를 생성합니다. 

BoardVO.java 를 생성합니다.

```java
package com.cali.myapp.vo;

public class BoardVO {
	private int no;
	private String subject;
	private String content;
	private String userid;
	private String ip;
	private int hit;
	private String writedate;
	
	@Override
	public String toString() {
		return "BoardVO [no=" + no + ", subject=" + subject + ", content=" + content + ", userid=" + userid + ", ip="
				+ ip + ", hit=" + hit + ", writedate=" + writedate + "]";
	}

	public int getNo() {
		return no;
	}

	public void setNo(int no) {
		this.no = no;
	}

	public String getSubject() {
		return subject;
	}

	public void setSubject(String subject) {
		this.subject = subject;
	}

	public String getContent() {
		return content;
	}

	public void setContent(String content) {
		this.content = content;
	}

	public String getUserid() {
		return userid;
	}

	public void setUserid(String userid) {
		this.userid = userid;
	}

	public String getIp() {
		return ip;
	}

	public void setIp(String ip) {
		this.ip = ip;
	}

	public int getHit() {
		return hit;
	}

	public void setHit(int hit) {
		this.hit = hit;
	}

	public String getWritedate() {
		return writedate;
	}

	public void setWritedate(String writedate) {
		this.writedate = writedate;
	}
	
	
}

```



BoardDAO.java 를 작성합니다.

```java
package com.cali.myapp.dao;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

import com.cali.myapp.vo.BoardVO;

@Mapper
@Repository
public interface BoardDAO {
	// 글목록
	public List<BoardVO> boardList();
	// 글등록
	public int boardWriteOk(BoardVO vo);
	// 글선택(글수정), 글내용보기
	public BoardVO getBoard(int no);
	// 글수정
	public int boardEditOk(BoardVO vo);
	// 글삭제
	public int boardDel(int no, String userid);
	// 조회수
	public void hitCount(int no);
}

```



BoardDAO 에 있는 내용을 BoardService 에 옮겨줍니다. 저장하고 나서 BoardServiceImpl 로 가서 @Override 해줍니다.

```java
package com.cali.myapp.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.cali.myapp.dao.BoardDAO;
import com.cali.myapp.vo.BoardVO;

@Service
public class BoardServiceImpl implements BoardService {
	@Autowired
	BoardDAO dao;

	@Override
	public List<BoardVO> boardList() {
		return dao.boardList();
	}

	@Override
	public int boardWriteOk(BoardVO vo) {
		return dao.boardWriteOk(vo);
	}

	@Override
	public BoardVO getBoard(int no) {
		return dao.getBoard(no);
	}

	@Override
	public int boardEditOk(BoardVO vo) {
		return dao.boardEditOk(vo);
	}

	@Override
	public int boardDel(int no, String userid) {
		return dao.boardDel(no, userid);
	}

	@Override
	public void hitCount(int no) {
		dao.hitCount(no);
	}

}

```



Top.jspf > boardController > boardMapper 순으로 작성합니다.

다음으로 board 폴더 아래에 boardList.jsp 파일을 하나 만들고 코드를 작성합니다. 



