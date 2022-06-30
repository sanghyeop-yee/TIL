# Database: MySQL 를 이용하여 Java DAO 에서 입력받기

> Thu Jun 30, 2022

---

[toc]

> DAO : Data Access Object -> 데이터베이스 처리하는 기능만 가지는 클래스



JDBC 를 이용하여 데이터베이스와 연결을 해보았고 DBConn, Select, Insert, Update, Class 를 각각의 클래스로 만들어서 사용해보았습니다. 

이번에는 하나의 DAO 클래스에서 Select, Insert, Update, Class 를 메소드로 만들어서 구현해보겠습니다. 



우선 Select 는 아래의 순서로 진행됩니다.

DAO 에서 Select 를 실행하면 ResultSet 을 리턴해줍니다. 각각의 레코드를 하나의 VO 로 만들어서 하나의 Collection 으로 담고 묶어서 가져오는 과정입니다. 

1. EmpVO : 변수들을 선언하는 VO 클래스를 선언
   * 사용할 변수들을 정의하고 Getter/Setter 생성

2. DBConn 클래스 만들고 상속하기
   * 데이터베이스 연결시 사용
3. EmpDAO 클래스에서 Select, Insert, Update, Class 메소드 생성
   * 데이터베이스 연동, 사용시 사용
4. EmpStart 클래스 생성
   * 변수를 가져올때 사용
     1. 메뉴 출력 메소드 생성
     2. if else 문으로 메뉴선택
     3. 종료 메소드 생성
     4. 회원목록 메소드 EmpList 생성
5. EmpDAO 클래스에서 empSelect 메소드 생성



## [Java Project] employeesOOP

### [Package] default package

#### [Class] EmpStart.java

```java
import java.util.List;
import java.util.Scanner;

import employeesOOP.EmpDAO;
import employeesOOP.EmpVO;

public class EmpStart {
	Scanner scan = new Scanner(System.in);
	EmpDAO dao = new EmpDAO();

	public EmpStart() {
		start();
		
	}
	public void start() {
		
		do {
			try {
				String menu = menuShow();
				if(menu.equals("1")) { // 회원목록
					empList();
				}else if(menu.equals("2")) { // 회원등록
					empAdd();
				}else if(menu.equals("3")) { // 회원수정
					empEdit();
				}else if(menu.equals("4")) { // 회원삭제
					empDel();
				}else if(menu.equals("5")) { // 종료
					empClose();
				}else if(menu.equals("6")) { // 회원검색
					empSearch();
				}
			}catch(Exception e) {
				System.out.println("Invalid menu.");
			}
		}while(true);
		
	}
	// 회원검색
	// 검색은 단순히 쿼리 select 과 동일하지만 where 구문이 추가 된것이다.
	public void empSearch() {
		System.out.print("Search by username -> ");
		String searchWord = scan.nextLine();
		// 출력 
		empPrint(dao.empSelect(searchWord));
		
	}
	// 회원목록 메소드
	public void	empList() {
		// 회원목록 DB 에서 선택할 수 있는 메소드를 호출
		// EmpDAO dao = EmpDAO.getInstance();
		// EmpDAO dao = new EmpDAO(); // 이렇게도 가능
		String searchWord = null;
		empPrint(dao.empSelect(searchWord));

	}
	// 목록출력
	public void empPrint(List<EmpVO> list) {
		for(int i=0; i<list.size(); i++) {
			EmpVO vo = list.get(i);
			System.out.printf("%-6d %-12s %-10s %-16s %-20s\n", 
					vo.getMem_id(), vo.getUsername(), vo.getDepart(), vo.getPhone(), vo.getEmail());
		}
	}
	
	// 회원정보 삭제
	public void empDel() {
		System.out.print("Delete mem_id -> ");
		int mem_id = Integer.parseInt(scan.nextLine());
		// vo 객체 생성 없이
		// EmpDAO dao = new EmpDAO();
		int result = dao.empDelete(mem_id);
		if(result>0) {
			System.out.println(mem_id+" is deleted.");
		}else {
			System.out.println("Failed to delete.");
		}
		
	}
	
	// 회원정보 수정
	public void empEdit() {
		EmpVO vo = new EmpVO();
		
		// 수정할 회원번호 입력받기
		System.out.print("Edit mem_id -> ");
		vo.setMem_id(Integer.parseInt(scan.nextLine()));
		
		// 수정할 항목 입력받
		System.out.print("Edit menu [1.phone, 2.depart, 3.email] -> ");
		// EmpVO 에 변수 하나 (fieldName) 추가하고 Setter/Getter 생성
		String editField = scan.nextLine();
		
		if(editField.equals("1")) {
			vo.setFieldName("phone");
			System.out.print("Enter a new phone -> ");
		}else if(editField.equals("2")) {
			vo.setFieldName("depart");
			System.out.print("Enter a new depart -> ");
		}else if(editField.equals("3")) {
			vo.setFieldName("email");
			System.out.print("Enter a new email -> ");
			
		}
		vo.setPhone(scan.nextLine());
		
		// EmpDAO dao = EmpDAO.getInstance();
		// EmpDAO 의 empUpdate 에서 int 로 반환된 값으로
		int cnt = dao.empUpdate(vo);
		if(cnt>0) {
			System.out.println("Succesfuly updated to "+vo.getPhone()+".");
		}else {
			System.out.println("Failed to update.");
		}
		
	}
	
	// 회원등록 메소드
	public void empAdd() {
		// 입력받은 데이터를 VO 저장하기 위해서 객체를 생성
		EmpVO vo = new EmpVO();
		System.out.print("mem_id -> ");
		vo.setMem_id(Integer.parseInt(scan.nextLine()));
		
		System.out.print("username -> ");
		vo.setUsername(scan.nextLine());
		
		System.out.print("depart -> ");
		vo.setDepart(scan.nextLine());
		
		System.out.print("phone -> ");
		vo.setPhone(scan.nextLine());
		
		System.out.print("email -> ");
		vo.setEmail(scan.nextLine());
		
		//EmpDAO dao = new EmpDAO();
		int cnt = dao.empInsert(vo);
		if(cnt>0) {
			System.out.println(vo.getUsername()+" is successfully added.");
		}else {
			System.out.println("Failed to add.");
		}
		
	}
	

	
	
	// 종료 메소드
	public void empClose() {
		System.exit(0);
	}
	
	// 메뉴 출력 메소드
	public String menuShow() {
		System.out.print("Menu [1.Member List, 2.Add, 3.Edit, 4.Delete, 5.End, 6.Search] -> ");
		return scan.nextLine();
	}

	public static void main(String[] args) {
		new EmpStart();

	}

}

```



### [Package] employeessOOP

#### [Class] DBConn.java

```java
package employeesOOP;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class DBConn {

	// 드라이버로딩 메소드
	static {
		try {
			Class.forName("com.mysql.cj.jdbc.Driver");
		}catch(Exception e) {
			e.printStackTrace();
		}
	}
	// 변수 
	// protected 로 외부에서 사용 안되고 반드시 상속받아서 쓰게만 허용
	protected Connection conn;
	protected PreparedStatement pstmt;
	protected ResultSet rs;
	protected String sql = null;
	protected final String URL = "jdbc:mysql://@127.0.0.1/multi";
	protected final String DB_ID = "root";
	protected final String DB_PWD = "password";
	
	// DB 연결 메소드
	protected void getConn() {
		try {
			conn = DriverManager.getConnection(URL, DB_ID, DB_PWD);
		}catch(Exception e) {
			e.printStackTrace();
		}
	}
	
	// DB 닫기 메소드 
	protected void getClose() {
		try {
			if(rs != null) rs.close();
			if(pstmt != null) pstmt.close();
			if(conn != null) conn.close();
			
		}catch(Exception e) {
			e.printStackTrace();
		}
	}
}

```



#### [Class] EmpDAO.java

```java
package employeesOOP;

import java.util.ArrayList;
import java.util.List;

public class EmpDAO extends DBConn{

	public EmpDAO() {
		
	}
	public static EmpDAO getInstance() { // 클래스를 객체화하기 
		return new EmpDAO();
	}
	// 회원목록
	public List<EmpVO> empSelect(String searchWord) { // arraylist 에 담김 회원정보를 리턴
		List<EmpVO> list = new ArrayList<EmpVO>();
		try {
			getConn();
			
			sql = "select mem_id, username, depart, phone, email from member ";
					if(searchWord != null) {
						sql += " where username like ? ";
					}
					sql += " order by mem_id";
			
			// select mem_id, username, depart, phone, email from member order by mem_id
			// select mem_id, username, depart, phone, email from member where username like ? order by mem_id		
			pstmt = conn.prepareStatement(sql);
			
			if(searchWord!=null) {
				pstmt.setString(1,  "%"+searchWord+"%");
			}
			
			////
			rs = pstmt.executeQuery();
			while(rs.next()) {
				// 회원 한명을 VO에 담기
				EmpVO vo = new EmpVO();
				vo.setMem_id(rs.getInt(1));
				vo.setUsername(rs.getString(2));
				vo.setDepart(rs.getString(3));
				vo.setPhone(rs.getString(4));
				vo.setEmail(rs.getString(5));
				
				// VO를 ArrayList (collection) 에 담기
				list.add(vo);
			}
			
		}catch(Exception e) {
			e.printStackTrace();
		}finally {
			getClose();
		}
		return list;  // ArrayList 에 담김 회원정보를 리턴
	}
	// 회원등록
	public int empInsert(EmpVO vo) {
		int result = 0;
		try {
			getConn();
			sql = "Insert into member(mem_id, username, depart, phone, email) "
					+ " values(?,?,?,?,?)";
			pstmt = conn.prepareStatement(sql);
			pstmt.setInt(1, vo.getMem_id());
			pstmt.setString(2, vo.getUsername());
			pstmt.setString(3, vo.getDepart());
			pstmt.setString(4, vo.getPhone());
			pstmt.setString(5, vo.getEmail());
			
			result = pstmt.executeUpdate();
			
		}catch(Exception e) {
			System.out.println(e.getMessage());
		}finally {
			getClose();
		}
		return result;
		
	}
	
	// 회원수정
	public int empUpdate(EmpVO vo) {
		int result=0;
		try {
			getConn();
			sql = "update member set "+vo.getFieldName()+"=? where mem_id=?";
			pstmt = conn.prepareStatement(sql);
			pstmt.setString(1, vo.getPhone()); // 첫번째 물음표는 문자고 vo.getPhone 에 값이 들어있다.
			pstmt.setInt(2, vo.getMem_id()); // 두번째 물음표는 정수고 vo.getMem_id 에 값이 들어있다.
			
			result = pstmt.executeUpdate();
			
		}catch(Exception e) {
			e.printStackTrace();
		}finally {
			getClose();
		}
		return result;
	}
	
	// 회원삭제
	public int empDelete(int mem_id) {
		int result=0;
		try {
			getConn();
			sql = "delete from member where mem_id = ?";
			
			pstmt = conn.prepareStatement(sql);
			pstmt.setInt(1, mem_id);
			
			result = pstmt.executeUpdate(); // 실행 후 리턴
			
		}catch(Exception e) {
			e.printStackTrace();
		}
		return result;
	}

}

```



#### [Class] EmpVO.java

```java
package employeesOOP;

public class EmpVO {
	private int mem_id;
	private String username;
	private String depart;
	private String phone;
	private String email;
	private String writedate;
	private String fieldName;

	public EmpVO() {
		
	}

	public int getMem_id() {
		return mem_id;
	}

	public void setMem_id(int mem_id) {
		this.mem_id = mem_id;
	}

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getDepart() {
		return depart;
	}

	public void setDepart(String depart) {
		this.depart = depart;
	}

	public String getPhone() {
		return phone;
	}

	public void setPhone(String phone) {
		this.phone = phone;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String getWritedate() {
		return writedate;
	}

	public void setWritedate(String writedate) {
		this.writedate = writedate;
	}

	public String getFieldName() {
		return fieldName;
	}

	public void setFieldName(String fieldName) {
		this.fieldName = fieldName;
	}
	
}

```

