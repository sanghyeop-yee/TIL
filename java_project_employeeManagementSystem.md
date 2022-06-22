# Employee Management System

> Wed Jun 22, 2022

---

[toc]

사원 관리 프로그램을 만들어봅시다.
기능의 메뉴는 목록, 등록, 수정, 삭제, 종료, 검색이 있습니다. 

수정(3번)을 입력하면 수정을 할 사원번호를 입력하도록 합니다. 이후에는 수정할 메뉴를 선택하도록 합니다. 

종료를 하면 하나의 파일을 만들어서 정보를 저장하며 검색은 사원명으로 합니다.



---

**사원목록**

| 사번 | 사원명 | 부서명   | 연락처       |
| ---- | ------ | -------- | ------------ |
| 1    | John   | Business | 650-804-1499 |

메뉴[1. 목록, 2. 등록, 3. 수정, 4. 삭제, 5. 종료, 6. 검색] 

사원번호 = 
사원명 =
부서명 =
연락처 =

---



### EmpVO

사원 한명의 정보를 저장하기 위해 만듭니다.

```java
package module;

import java.io.Serializable;

public class EmpVO extends Object implements Serializable {
	// 사원 한명의 정보를 저장할 VO
	// 캡슐화 (getter / setter 생성하기)
	private int empno;
	private String empName;
	private String department;
	private String tel;

	
	
	public EmpVO() {
		
	}
	// 생성자 만들기
	public EmpVO(int empno, String empName, String department, String tel) {
		this.empno = empno;
		this.empName = empName;
		this.department = department;
		this.tel = tel;
		
	}
	/////////////
	@Override
	public String toString() {
		return empno+"\t"+empName+"\t"+department+"\t"+tel;
	}
	
	
	/////////////

	public int getEmpno() {
		return empno;
	}

	public void setEmpno(int empno) {
		this.empno = empno;
	}

	public String getEmpName() {
		return empName;
	}

	public void setEmpName(String empName) {
		this.empName = empName;
	}

	public String getTel() {
		return tel;
	}

	public void setTel(String tel) {
		this.tel = tel;
	}

	public String getDepartment() {
		return department;
	}

	public void setDepartment(String department) {
		this.department = department;
	}

}
```



### EmpDataSet

사번을 Key 로 사용합니다. Generic 을 사용하기 때문에 Integer 로 데이터 타입을 지정합니다.

```java
package module;

import java.io.File;
import java.io.FileInputStream;
import java.io.ObjectInputStream;
import java.util.HashMap;

public class EmpDataSet {
	// 사원정보를 저장할 컬렉션을 선언
	public static HashMap<Integer, EmpVO> empList = new HashMap<Integer, EmpVO>();

	public EmpDataSet() {
		
	}
	public static void dataSet() {
		// 파일에 있는 Object 를 구하여 HashMap 에 셋팅
		try {
			
			File f = new File("/Users/sanghyeop/testFolder", "employee.txt");
			if(f.exists()) { // 파일이 있을때만 작업 가능
				FileInputStream fis = new FileInputStream(f); // FileInputStream은 File 객체가 필요.
				ObjectInputStream ois = new ObjectInputStream(fis); //InputStream 객체를 넣어줌.
				
				EmpDataSet.empList = (HashMap)ois.readObject(); // 초기회원목록
			}
			
		}catch(Exception e) {
			e.printStackTrace();
		}
	}

}
```



### EmpStart

java.lang.System 에 있는 exit 을 사용해서 현재 실행중인 프로그램을 종료할 수 있습니다. static 이기 때문에 `System.exit(0);` 으로 호출가능 합니다.

![image-20220622140950655](/Users/sanghyeop/Library/Application Support/typora-user-images/image-20220622140950655.png)



hashmap 이라는 객체를 파일로 써야한다면 `ObjectOutputStream` 을 사용합니다. 

```java
package employees;

import java.io.File;
import java.io.FileOutputStream;
import java.io.ObjectOutputStream;
import java.util.Collection;
import java.util.Iterator;
import java.util.Scanner;

import module.EmpDataSet;
import module.EmpVO;

public class EmpStart {
	Scanner scan = new Scanner(System.in);

	public EmpStart() {
		// 현재 등록되어있는 사원목록
		EmpDataSet.dataSet(); // 초기 데이터 셋팅
		
		// do while 로 반복실행
		do {
			try {
				System.out.print(menu());
				//String inMenu = scan.nextLine(); // 숫자 다음 엔터를 인식시키기 위해 nextInt 가 아닌 nextLine
				int inMenu = Integer.parseInt(scan.nextLine());
				if(inMenu==5) { // 종료
					empStop();
				}else if(inMenu==1){ // 사원목록(모든사원)
					empListOutput();
				}else if(inMenu==2) { // 사원등록
					empInsert();
				}else if(inMenu==3) { // 사원수정
					empEdit();
				}else if(inMenu==4){ // 사원삭제
					empDel();
				}else if(inMenu==6) { // 사원 이름으로 검색
					getNameSearch();
				}else {
					throw new Exception();
				}
			}catch(Exception e) {
				System.out.println("Invalid menu.");
			}
			
		}while(true);
	}
	// 사원검색
	public void getNameSearch() {
		System.out.print("Enter Employee Name = ");
		String searchName = scan.nextLine();
		
		// searchName 이 vo 에 있는지 확인하기
		titlePrint(); // 제목출력
		// vo
		Collection<EmpVO> list = EmpDataSet.empList.values();
		Iterator<EmpVO> ii = list.iterator();
		
		int cnt = 0;
		while(ii.hasNext()) {
			EmpVO v = ii.next(); // 끄집어내기
			if(v.getEmpName().equals(searchName)) {
				System.out.println(v.toString());
				cnt++;
			}
		}
		System.out.println("Found "+ cnt + " results.");

	}
	
	
	// 사원삭제
	public void empDel() {
		System.out.print("Enter Employee ID = ");
		int empno = Integer.parseInt(scan.nextLine());
		EmpDataSet.empList.remove(empno); // 객체지워짐
		System.out.println("Employee ID "+empno+" is deleted.");
	}
	
	// 사원수정
	public void empEdit() {
		// 어떤사원을 수정할것인지 입력받는다.
		System.out.print("Enter Employee ID = ");
		int editEmpno = Integer.parseInt(scan.nextLine());
		// 어떤정보를 수정할것인지 입력받는다.
		System.out.print("Edit [1.Department, 2.Telephone] = ");
		String editMenu = scan.nextLine();
		
		// 1번이면 부서, 2번이면 연락처 수정
		if(editMenu.equals("1")) { // 부서 수정
			departmentEdit(editEmpno);
		}else if(editMenu.equals("2")) { // 연락처 수정
			telEdit(editEmpno);
		}else {
			System.out.println("Invalid menu.");
		}
		
	}
	// 부서명 수정 메소드
	public void departmentEdit(int empno) {
		EmpVO vo = EmpDataSet.empList.get(empno); // 수정할 사원번호 가져오기
		System.out.print("Enter new department = ");
		String editDepartName = scan.nextLine();
		vo.setDepartment(editDepartName); // 부서명이 변경됨.
		System.out.println(vo.getEmpName()+"'s department is updated to " + vo.getDepartment() + ".");
		
	}
	// 연락처 수정 메소드
	public void telEdit(int empno) {
		EmpVO vo = EmpDataSet.empList.get(empno);
		System.out.print("Enter new telephone = ");
		vo.setTel(scan.nextLine());
		System.out.println(vo.getEmpName()+"'s telephone is updated to "+ vo.getTel() + ".");
	}
	
	// 사원목록
	public void empListOutput() {
		titlePrint(); // 제목출력
		// 앱의 모든 value 를 (vo 객체) 를 구하여 목록을 출력한다.
		Collection<EmpVO> emp = EmpDataSet.empList.values();
		Iterator<EmpVO> i = emp.iterator();
		
		while(i.hasNext()) {
			EmpVO v = i.next();
			System.out.println(v.toString());
		}
		
	}
	
	public void titlePrint() { // 사원목록 제목출력
		System.out.println("EmpID\tEmpName\tDepartment\tTelephone");
	}
	
	// 사원등록
	public void empInsert() {
		// 입력을 받아서 사원한명의 정보를 넣기위해 만든 empVO 넘기
		EmpVO vo = new EmpVO(); // 입력받은 사원정보를 저장할 vo 객체 생성
		// EmployeeID
		System.out.print("Employee ID = ");
		vo.setEmpno(Integer.parseInt(scan.nextLine())); // vo 의 setter 호출, 문자로 받아서 숫자로 변환
		// EmployeeName
		System.out.print("Employee Name = ");
		vo.setEmpName(scan.nextLine());
		// Department
		System.out.print("Department = ");
		vo.setDepartment(scan.nextLine());
		// Telephone
		System.out.print("Telephone = ");
		vo.setTel(scan.nextLine());
		
		// 컬렉션에 vo를 추가 (== 사원 한명 추가)
		EmpDataSet.empList.put(vo.getEmpno(), vo);
		System.out.println("Added a new employee.");
	}
	
	// 종료
	public void empStop() {
		try {
			// 사원정보가 있는 empList를 파일로 저장하고 프로그램 종료
			File f = new File("/Users/sanghyeop/testFolder", "employee.txt");
			FileOutputStream fos = new FileOutputStream(f);
			ObjectOutputStream oos = new ObjectOutputStream(fos);
			
			oos.writeObject(EmpDataSet.empList);
			
			oos.close(); // 쓰기가 끝나면 close
			fos.close();
			
		}catch(Exception e) {
			System.out.println("Exception occured when saving employees in a file! " + e.getMessage());
			e.printStackTrace();
		}
		System.exit(0);
	}
	
	public String menu() {
		return "Menu [1.Employee List, 2.Add, 3.Edit, 4.Delete, 5.End, 6.Search] = ";
	}

	public static void main(String[] args) {
		new EmpStart();

	}

}
```





