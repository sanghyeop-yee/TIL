# 예외처리

> Wed Jun 15, 2022

------



>  소프트웨어로 처리할 수 있는 오류를 예외 (Exception), 처리할 수 없는 오류를 에러라고 한다.



다음과 같이 정수를 입력받아 출력하는 프로그램이 있습니다.

```java
import java.util.Scanner;

public class ExceptionTest1 {
	public ExceptionTest1() {
		
	}
	public void start() {
		Scanner scan = new Scanner(System.in);
		System.out.print("첫번째정수 = ");
		int first = scan.nextInt();
		System.out.print("두번째정수 = ");
		int second = scan.nextInt();
		
		System.out.printf("first = %d, second = %d\n", first, second);
	}

	public static void main(String[] args) {
		new ExceptionTest1().start();

	}

}
```



정수가 아닌 문자열을 입력하면 다음과 같은 오류가 발생합니다.

```
첫번째정수 = d
Exception in thread "main" java.util.InputMismatchException
	at java.base/java.util.Scanner.throwFor(Scanner.java:939)
	at java.base/java.util.Scanner.next(Scanner.java:1594)
	at java.base/java.util.Scanner.nextInt(Scanner.java:2258)
	at java.base/java.util.Scanner.nextInt(Scanner.java:2212)
	at basic04_exception.ExceptionTest1.start(ExceptionTest1.java:12)
	at basic04_exception.ExceptionTest1.main(ExceptionTest1.java:20)

```



[java.util.InputMismatchException](https://docs.oracle.com/javase/8/docs/api/index.html) 패키지를 자바 문서에서 찾아봅시다.

다음의 두가지 방법으로 **예외처리**를 할 수 있습니다.

1. try catch 문 이용하기
2. 메소드 (생성자메소드, 메소드) 를 이용하기



위에서 보면 오류가 발생할 가능성이 있는 곳은 `scan.nextInt()` 코드입니다.

```java
try{
  예외가 발생할 가능성이 있는 코드
} catch(예외종류){
  예외가 발생했을때 처리하는 곳
}
```



### InputMismatchException

정수가 아닌 문자열을 입력했을때 오류를 잡으려면 아래와 같이 작성합니다.

```java
import java.util.InputMismatchException;
import java.util.Scanner;

public class ExceptionTest1 {
	public ExceptionTest1() {
		
	}
	public void start() {
		Scanner scan = new Scanner(System.in);
		
		try { // 예외가 발생가능한 코드 + 예외 발생가능성이 없는 코드
			System.out.print("첫번째정수 = ");
			int first = scan.nextInt();
			System.out.print("두번째정수 = ");
			int second = scan.nextInt();
			
			System.out.printf("first = %d, second = %d\n", first, second);
		}catch(InputMismatchException ime) { // 예외발생시 처리하는 부분
			System.out.println("정수를 입력하셔야 합니다.");
		}
		System.out.println("End..."); // 에외가 없거나 예외발생시 처리후 이곳으로 
	}

	public static void main(String[] args) {
		new ExceptionTest1().start();

	}

}
```



오류 메시지를 보고싶다면 아래 두가지를 이용해서 확인할 수 있습니다.

`ime.getMessage()`

`ime.printStackTrace()`

```java
}catch(InputMismatchException ime) { // 예외발생시 처리하는 부분
			System.out.println("정수를 입력하셔야 합니다.");
			// ime 변수 발생한 예외 객체가 담겨있다.
			
			System.out.println(ime.getMessage());
			ime.printStackTrace();
```



### ArithmeticException

다음으로 0으로 나누는 경우의 예외종류를 잡으려면 다음과 같이 작성합니다.

```java
}catch(ArithmeticException ae) {
  System.out.println("second 변수를 값을 0으로 입력했을때");
  System.out.println(ae.getMessage());
}
```



### ArrayIndexOutOfBoundsException

아래의 예시처럼 배열의 길이가 6개인데 index 를 5개만 쓴 경우의 예외종류를 잡으려면 다음과 같이 작성합니다.

```java
int data[] = {7,9,2,4,8,5};
			for(int i=0; i<=data.length; i++) {
				System.out.println("data["+i+"]="+ data[i]);
```



```java
}catch(ArrayIndexOutOfBoundsException aibe) {
	System.out.println("배열을 index가 잘못되었습니다.");
	System.out.println(aibe.getMessage());
}
```



### finally

예외가 발생하든 안하든 무조건 실행합니다.

```java
try{
  예외가 발생할 가능성이 있는 코드
} catch(예외종류){
  예외가 발생했을때 처리하는 곳
} finally {
	예외가 발생하든 안하든 무조건 실행
}
```



### Exception

예외가 너무 많아진다면 예외종류가 너무 많아지겠죠?

만약 예외가 발생했을때 처리할 일들이 **같다면** 한곳에서 처리가 가능합니다.

공통으로 상속받고 있는 상위 클래스가 `Exception` 이기 때문에 묶어서 처리가 가능합니다.

단, `Exception` catch 문은 **마지막**에 사용해야 합니다.

```java
import java.util.Scanner;

public class ExceptionTest2 {
	Scanner scan = new Scanner(System.in);
	
	public ExceptionTest2() {
		start();
	}
	public void start() {
		try {
			// InputMismatchException 
			System.out.print("첫번째정수 = "); 
			int first = scan.nextInt();
			// InputMismatchException
			System.out.print("두번째정수 = "); 
			int second = scan.nextInt();
			
			System.out.printf("first = %d, second = %d\n", first, second);
			
			// ArithmeticException
			int result = first / second; 
			System.out.println(first+"/"+second+"="+result);
			
			// ArrayIndexOutOfBoundsException
			int data[] = {7,9,2,4,8,5};
			for(int i=0; i<= data.length; i++) { 
				System.out.println("data["+i+"]="+ data[i]); 
			}
		}catch(ArrayIndexOutOfBoundsException ae) {
			System.out.println("배열의 인덱스를 잘못 사용하셨습니다.");
		}catch(Exception e) {
			System.out.println("숫자를 다시 입력하세요");
		}
	}

	public static void main(String[] args) {
		new ExceptionTest2();
	}
}
```



### 메소드에서 예외처리

예외처리를 메소드에서 사용할때는 `throws` 를 사용해서 처리합니다.

```java
import java.util.InputMismatchException;
import java.util.Scanner;

public class MethodException {

	public MethodException() throws InputMismatchException, ArithmeticException{
		start();
	}
	public void start() throws InputMismatchException, ArithmeticException{
		sum();
	}
	public void sum() throws InputMismatchException, ArithmeticException{
		int sum=0;
		for(int i=1; i<=100; i++) {
			sum += i;
		}
		System.out.println("sum="+sum);
		inData();
	}
	public void inData() throws InputMismatchException, ArithmeticException{ 
		Scanner s = new Scanner(System.in);
		System.out.println("정수입력=");
		int i = s.nextInt();
		result(i);
	}
	public void result(int i) {
		int output = 100 / i;
		System.out.println("output="+ output);
	}

	public static void main(String[] args) {
		try {
			new MethodException(); 
		}catch(InputMismatchException ie) {
			System.out.println("InputMismatchException가 발생함.");
		}catch(ArithmeticException a) {
			a.printStackTrace();
		}
		 
	}

}
```



### 예외 만들기

예를 들어 1~100 사이가 아니면 예외를 발생시키려면?

1. Exception 상속받아 만들기

[java.lang.Exception](https://docs.oracle.com/javase/8/docs/api/index.html) 을 자바 문서에서 확인해봅시다.

```java
// 예외클래스를 생성하기 위해서는 Exception 상속받아 만들수 있다.
public class MyException extends Exception{

	public MyException() {
		// 상위클래스인 Exception 의 생성자메소드를 호출하여 에러 메시지를 셋팅한다.
		super("1~100사이의 값의 범위를 벗어났습니다.");
	}
	public MyException(String msg) {
		super(msg);
	}

}
```

```java

import java.util.Scanner;

public class MyExceptionTest {

	public MyExceptionTest() {
		Scanner scan = new Scanner(System.in);
		try {
			System.out.print("숫자=");
			int n = scan.nextInt();
			// 입력받은 값이 1~100사이의 값이 아니면 MyException 예외 발생
			if(n<1 || n>100) {
				// throw 는 강제로 예외발생 시키는 키워드 
				// throw new MyException();
				throw new MyException("1부터 100사이 데이터여야 합니다.");
			}
			sum(n);
		}catch(MyException me) {
			System.out.println(me.getMessage());
		}
	}
	public void sum(int n) {
		int tot=0;
		for(int i=1; i<=n; i++) {
			tot += i;
		}
		System.out.println("1~"+n+"까지의 합은 "+tot);
	}
	public static void main(String[] args) {
		new MyExceptionTest();

	}

}
```

