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

다음의 두가지 방법으로 예외처리를 할 수 있습니다.

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



### ArimeticException

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





예외가 너무 많아진다면 예외종류가 너무 많아지겠죠?

만약 예외가 발생했을때 처리할 일들이 **같다면** 한곳에서 처리가 가능합니다.



C -> B -> A -> Object

하위 클래스 객체는 상위 클래스 객체에 들어갈 수 있고

상위 클래스 객체는 하위 클래스 변수에 넣으려면 안됩니다.

하위를 상위에 넣다가 다시 하위로 가지고 오는것은 가능합니다.

하위 클래스를 상위 클래스에 대입하는것은 가능합니다.