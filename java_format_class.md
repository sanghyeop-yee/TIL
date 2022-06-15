# Format 클래스

> Wed Jun 15, 2022

---

> Currency 를 표현하듯이 정수 세자리마다 쉼표를 넣고 싶다면?



### Decimal Format

[java.text.DecimalFormat](https://docs.oracle.com/javase/8/docs/api/index.html) 을 자바 문서에서 확인해봅시다.

```java
import java.text.DecimalFormat;

public class FormatTest {

	public FormatTest() {
		// 정수 (int) 를 문자열로 만들기
		int won = 5475483; // 5,475,483원
		// won 의 값을 화폐단위로 만들기 
		DecimalFormat fmt = new DecimalFormat("#,###원");
		String wonResult = fmt.format(won);
		System.out.println(wonResult);
	}

	public static void main(String[] args) {
		new FormatTest();

	}

}
```



### 날짜 패턴

`SimpleDateFormat dateFmt = new SimpleDateFormat("yyyy.MM.dd(E) hh:mm:ss a");`

```java
import java.text.SimpleDateFormat;
import java.util.Calendar;

public class FormatTest {

	public FormatTest() {
		// 날짜 패턴
		// 2022-06-15(Wed) 04:52:22 PM
		Calendar now = Calendar.getInstance();
		
		SimpleDateFormat dateFmt = new SimpleDateFormat("yyyy.MM.dd(E) hh:mm:ss a");
		String dateTxt = dateFmt.format(now.getTime());
		System.out.println(dateTxt);

	}
	public static void main(String[] args) {
		new FormatTest();
	}
}
```



### Message Format

변수의 값을 이용하여 문자열로 만들어주는 포멧

`String msg = MessageFormat.fotmat(String pattern, Object... arguments);`

```java
import java.text.MessageFormat;

public class FormatTest {

	public FormatTest() {
		// 변수의 값을 이용하여 문자열로 만들어주는 포멧
		/// Name: John, Tel: 650-804-1459, Addr: 570 21st Street, Oakland CA  
		String name = "John";
		String tel = "650-804-1459";
		String addr = "570 21st Street, Oakland CA 94612";
		String msg = MessageFormat.format("Name: {0}, Tel: {1}, Address: {2}", name, tel, addr);
		System.out.println(msg);
		
	}

	public static void main(String[] args) {
		new FormatTest();
	}
}
```

