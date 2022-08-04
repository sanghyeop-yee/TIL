

# Springboot: Board / Data 

> Thu Aug 4, 2022

---



이번에는 웹사이트에 파일 업로드를 하는 방법을 배워봅시다.



메이븐저장소에서 [Apache Commons FileUpload](https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload) » [1.4](https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload/1.4) 복사해와서 build.gradle 에 붙여넣고 Refresh Gradle Project 를 해줍니다. 

환경설정을 위해 application.yml 에 다음과 같이 지정하여 줍니다.

```xml
Servlet: # fileupload
  multipart:
    max-file-size: 20MB
    enabled: true
```





다음은 DB 설계를 할게요. 

보통 업로드할 파일의 갯수가 미리 정해져 있지 않다면 filename DB 를 따로 설계합니다.



파일 업로드를 하고 form request 를 하고 마지막으로 DB 등록을 합니다.

DB 등록을 하기 위해 dataInsert 라는 DAO 를 생성 후 Service, ServiceImpl 에서 차례대로 생성해줍니다. 마지막 Mapper 으로 가서 쿼리를 작성합니다.



#### DataDAO.java

```java
package com.cali.myapp.dao;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

import com.cali.myapp.vo.DataVO;

@Mapper
@Repository
public interface DataDAO {
	public List<DataVO> dataAllSelect(); // 글목록
	public int dataInsert(DataVO vo); // 
	public int downCount(int no); // 다운로드 횟수 증가
	public int newDownCount(int no); // 새로운 다운로드 회수 선택하기
	public int hitCount(int no); // 
	public DataVO dataSelect(int no); // 
}

```





#### DataService.java

```java
package com.cali.myapp.service;

import java.util.List;

import com.cali.myapp.vo.DataVO;

public interface DataService {
	public List<DataVO> dataAllSelect();
	public int dataInsert(DataVO vo); 
	public int downCount(int no); // 다운로드 횟수 증가
	public int newDownCount(int no); // 새로운 다운로드 회수 선택하기
	public int hitCount(int no); // 
	public DataVO dataSelect(int no); // 
}

```





#### DataServiceImpl.java

```java
package com.cali.myapp.service;

import java.util.List;

import javax.inject.Inject;

import org.springframework.stereotype.Service;

import com.cali.myapp.dao.DataDAO;
import com.cali.myapp.vo.DataVO;

@Service
public class DataServiceImpl implements DataService {
	@Inject
	DataDAO dao;
	
	@Override
	public List<DataVO> dataAllSelect(){
		return dao.dataAllSelect();
	}

	@Override
	public int dataInsert(DataVO vo) {
		return dao.dataInsert(vo);
	}

	@Override
	public int downCount(int no) {
		return dao.downCount(no);
	}

	@Override
	public int newDownCount(int no) {
		return dao.newDownCount(no);
	}

	@Override
	public int hitCount(int no) {
		return dao.hitCount(no);
	}

	@Override
	public DataVO dataSelect(int no) {
		return dao.dataSelect(no);
	}
}

```



#### DataVO.java

```java
package com.cali.myapp.vo;

public class DataVO {
	private int no;
	private String title;
	private String content;
	private String userid;
	private int downcount;
	private int hit;
	private String writedate;
	private String filename1;
	private String filename2;
	
	@Override
	public String toString() {
		return "DataVO [no=" + no + ", title=" + title + ", content=" + content + ", userid=" + userid + ", downcount="
				+ downcount + ", hit=" + hit + ", writedate=" + writedate + ", filename1=" + filename1 + ", filename2="
				+ filename2 + "]";
	}

	public int getNo() {
		return no;
	}

	public void setNo(int no) {
		this.no = no;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
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

	public int getDowncount() {
		return downcount;
	}

	public void setDowncount(int downcount) {
		this.downcount = downcount;
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

	public String getFilename1() {
		return filename1;
	}

	public void setFilename1(String filename1) {
		this.filename1 = filename1;
	}

	public String getFilename2() {
		return filename2;
	}

	public void setFilename2(String filename2) {
		this.filename2 = filename2;
	}
	
	
	
}

```



#### dataMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cali.myapp.dao.DataDAO">
	<select id="dataAllSelect" resultType="DataVO">
		select no, title, userid, downcount, hit, date_format(writedate, '%m-%d') writedate, filename1, filename2
		from data
		order by no desc
	</select>
	<insert id="dataInsert">
		insert into data(title, content, userid, filename1, filename2)
		values(#{title}, #{content}, #{userid}, #{filename1}, #filename2})
	</insert>
	<update id="downCount">
		update data set downcount=downcount+1 where no=${param1}
	</update>
	<select id="newDownCount" resultType="int">
		select downcount from data where no=${param1}
	</select>
	<update id="hitCount">
		update data set hit=hit+1 where no=${param1}
	</update>
	<select id="dataSelect" resultType="DataVO">
		select no, title, content, hit, downcount, userid, filename1, filename2, writedate
		from data
		where no=${param1}
	</select>

</mapper>
```

