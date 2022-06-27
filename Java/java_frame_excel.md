# Java 엑셀로 쓰기/읽기

> Wed Jun 22, 2022

----

 Excel 로 내용을 읽기쓰기를 하기위해서는 framework이 필요합니다.

 jakarta.apache.org 의 왼쪽카테고리에서 POI 를 선택하여 다운합니다.



### ExcelWriter

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

import org.apache.poi.hssf.usermodel.HSSFCell;
import org.apache.poi.hssf.usermodel.HSSFRow;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;

/*
 	Excel 로 내용을 읽기쓰기를 하기위해서는 framework이 필요하다.
 	POI 다운로드
 	jakarta.apache.org 의 왼쪽카테고리에서 POI 를 선택
 */
public class ExcelWriter {

	public ExcelWriter() {
		// 엑셀로 쓰기 
		// 1. workbook 생성
		HSSFWorkbook workbook = new HSSFWorkbook();
		
		// 2. sheet 생성
		HSSFSheet sheet1 = workbook.createSheet("Member List");
		HSSFSheet sheet2 = workbook.createSheet();
		
		// 3. 행(row) 생성
		HSSFRow row0 = sheet1.createRow(0); // 0행 생성
		
		// 4. cell 생성
		HSSFCell cell0 = row0.createCell(0); // 0열 생성
		cell0.setCellValue("Id");
		
		row0.createCell(1).setCellValue("Name");
		row0.createCell(2).setCellValue("Phone");
		
		HSSFRow row1 = sheet1.createRow(1);
		row1.createCell(0).setCellValue(1);
		row1.createCell(1).setCellValue("John");
		row1.createCell(2).setCellValue("010-9234-1234");
		
		HSSFRow row2 = sheet1.createRow(2);
		row2.createCell(0).setCellValue(2);
		row2.createCell(1).setCellValue("MJ");
		row2.createCell(2).setCellValue("010-1234-4939");
		
		HSSFRow row3 = sheet1.createRow(3);
		row3.createCell(0).setCellValue(3);
		row3.createCell(1).setCellValue("Andrew");
		row3.createCell(2).setCellValue("010-1234-5493");
		
		// 파일로 쓰기
		try {
			File f = new File("/Users/sanghyeop/testFolder/member.xls");
			FileOutputStream fos = new FileOutputStream(f); // 파일스트림은 파일객체가 필요
			workbook.write(fos);
		}catch(FileNotFoundException e) {
			System.out.println("File not found ->"+e.getMessage());
		}catch(IOException ie) {
			System.out.println("IOException ->"+ie.getMessage());
		}
		System.out.println("Completed excel write!");
	}

	public static void main(String[] args) {
		new ExcelWriter();

	}

}

```



### ExcelReader

```java
import java.io.File;
import java.io.FileInputStream;

import org.apache.poi.hssf.usermodel.HSSFCell;
import org.apache.poi.hssf.usermodel.HSSFRow;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.poifs.filesystem.POIFSFileSystem;

public class ExcelRead {

	public ExcelRead() {
		
	}
	public void start() {
		try {
			// inputstream 만들기
			File f = new File("/Users/sanghyeop/testFolder", "member.xls");
			FileInputStream fis = new FileInputStream(f);
			// 엑셀 파일을 객체 생성하기
			POIFSFileSystem poi = new POIFSFileSystem(fis);
			
			// 1. workbook 구하기
			HSSFWorkbook book = new HSSFWorkbook(poi); // 기존의 workbook 가져오기
			// 시트수 구하기
			int sheetCnt = book.getNumberOfSheets();
			System.out.println("Sheet counts -> "+ sheetCnt);
			
			// 2. 회원목록 시트 객체 얻어오기
			HSSFSheet sheet = book.getSheet("Member List"); // book.getSheetAt(0)
			// 행의 수 구하기
			int rowCnt = sheet.getPhysicalNumberOfRows();
			System.out.println("Row counts -> "+ rowCnt);

			System.out.println("----------------------------------------");
			
			
			System.out.println("Id\tName\tPhone");
			
			for(int idx=1; idx<rowCnt; idx++) { // 1,2,3
				HSSFRow row = sheet.getRow(idx);
				
				// 셀수를 구하기
				int cellCnt = row.getPhysicalNumberOfCells();
				
				for(int i=0; i<cellCnt; i++) { // 0,1,2
					HSSFCell cell = row.getCell(i);
					if(i==0) { // 번호일때
						int num = (int)row.getCell(i).getNumericCellValue(); // 숫자 double
						System.out.print(num+"\t");
					}else { // 이름, 전화번호일때
						String str = row.getCell(i).getStringCellValue(); // 문자 String
						System.out.print(str+"\t");
					}
				}
				System.out.println();
				
			}
			
		}catch(Exception e) {
			e.printStackTrace();
		}
	}

	public static void main(String[] args) {
		new ExcelRead().start();

	}

}

```

