# 입출력 (Input/Output)

> Fri Jun 17, 2022

---

[toc]

입력에 관련된 자바 클래스들은 Import Reader.

출력에 관련된 자바 클래스들은 Writer/Output Reader.

Java.io 자바 문서에서 확인해봅시다. 



Stream 은 데이터가 지나가는 길이라는 의미입니다.

추상클래스는 추상메소드 (아직 완성하지 않은) 를 가지고 있는 클래스입니다.

추상클래스는 객체생성을 할 수 없습니다.



### InputStream

> 한번에 1 byte 씩 가져옴



`InputStream in = System.in;`

`int inData = in.read();`

* try catch 구문으로 IOException 을 처리해주어야 합니다.

```java
import java.io.IOException;
import java.io.InputStream;

public class InputStreamTest {

	public InputStreamTest() {
		
	}
	public void start() {
		
		// input, output
		// input(byte), reader(문자) 단위로 읽어 온다.
		try {
			// byte 단위 문자를 입력받는다. 
			InputStream in = System.in;
			
			/*
			while(true) {
				int inData = in.read(); // -1 : 읽은값이 없을때 리턴
				if(inData==-1) break;
				System.out.println((char)inData);
			}*/
			
			/* read(byte[] a) */
			byte data[] = new byte[10];
			int cnt = in.read(data);
			System.out.println("cnt->"+cnt);
			System.out.println("text->"+ new String(data, 0, cnt-2));
			
		}catch(IOException ie) {
			System.out.println("Input error occured");
		}
	}

	public static void main(String[] args) {
		new InputStreamTest().start();

	}

}
```



### BufferedReader

>  한줄씩 읽어오려면?
>
> BufferedReader 는 버퍼에 입력문자를 저장한후 1줄씩 읽어올수 있다.
>
> InputStreamReader: 문자 단위 입력하는 클래스

** Buffer = 메모리

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class BufferedReaderTest {

	public BufferedReaderTest() {
		
	}
	public void start() {
		try {
			// 버퍼에 입력문자를 저장한후 1줄씩 읽어올수 있다.
			InputStreamReader isr = new InputStreamReader(System.in);
			BufferedReader br = new BufferedReader(isr);
			
			// 읽은 데이터가 없을때 null을 리턴해준다. 
			String str = br.readLine();
			System.out.println(str);
			
		}catch(Exception e) {
			e.printStackTrace();
		}
	}

	public static void main(String[] args) {
		new BufferedReaderTest().start();

	}

}
```



### File

> 파일에 있는 내용을 읽어서 가져오려면?
>
> 드라이브, 폴더, 파일에 관련한 정보를 사용하기 위해서는 File 객체를 생성하여야 합니다.

[Java.io.file](https://docs.oracle.com/javase/8/docs/api/index.html) 자바 문서에서 확인해봅시다.



`File(File parent, String child)`

`File(String parent, String child)`

`File(String pathname)`



```java
import java.io.File;

public class FileTest {

	public FileTest() {
		
	}
	public void start() {
		// 드라이브, 폴더, 파일에 관련한 정보를 사용하기 위해서는 File 객체를 생성하여야 한다.
		/*
		 	File(File parent, String child)
		 	File(String parent, String child)
		 	
		 	File(String pathname)
		 */
		File f1 = new File("/Users/myname");
		File f2 = new File("/Users/myname/TIL");
		File f3 = new File("/Users/myname/TIL/java_io.md");
		
		File f4 = new File(f2, "java_io.md");
		
		System.out.println(f2.exists());
		System.out.println(f3.exists());
	}

	public static void main(String[] args) {
		new FileTest().start();

	}

}
```



### FileWriter

콘솔에서 문자열을 줄단위로 입력받아 파일로 쓰기

```java
import java.io.BufferedReader;
import java.io.File;
import java.io.FileWriter;
import java.io.InputStreamReader;

public class FileWriterTest {

	public FileWriterTest() {
		
	}
	public void start() {
		// 콘솔에서 문자열을 줄단위로 입력받아 파일로 쓰기
		try {
			// BufferedReader
			BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
			System.out.print("Enter text: ");
			String inData = br.readLine();
			
			File file = new File("/Users/myname/test", "outputTest.java");
			FileWriter fw = new FileWriter(file);
			
			fw.write(inData, 0, inData.length());
			
			fw.close();
			
		}catch(Exception e) {
			System.out.println("Exceeption occured.");
		}
		System.out.println("Program ended");
		
	}

	public static void main(String[] args) {
		new FileWriterTest().start();

	}

}

```



하나의 디렉토리에서 다른 디렉토리로 복사를 하는 방법

```java
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileCopy {

	public FileCopy() {
		
	}
	public void start() {
		// 파일복사
		// /Users/sanghyeop/eclipse-workspace/basic01/src/GuGuDan.java
		// /Users/sanghyeop/test/GuGuDan.java
		File orgFile = new File("/Users/sanghyeop/eclipse-workspace/basic01/src/GuGuDan.java");
		File tarFile = new File("/Users/sanghyeop/test/GuGuDan.java");
		
		try {
			// 바이트수 만큼 한번에 orgFile 파일의 내용을 읽어 tarFile 로 쓰기를 한다.
			FileInputStream fis = new FileInputStream(orgFile);
			FileOutputStream fos = new FileOutputStream(tarFile);
			
			// 파일의 내용을 읽어서 저장할 배열
			byte[] sourceCode = new byte[(int)orgFile.length()];
			
			// 읽어온 바이트수를 리턴해준다. 
			int cnt = fis.read(sourceCode);
			
			// 쓰기
			fos.write(sourceCode, 0, cnt);
			
			fis.close();
			fos.close();
			
		}catch(FileNotFoundException fnfe) {
			System.out.println("File not found. -> "+ fnfe.getMessage());
		}catch(IOException ie) {
			System.out.println("I/O error occured -> "+ ie.getMessage());
		}
	}

	public static void main(String[] args) {
		new FileCopy().start();

	}

}
```

