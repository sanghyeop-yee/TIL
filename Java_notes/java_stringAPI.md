# String API

> Fri Jun 10, 2022

---

> String : 연산이 적은 데이터 스레드 가능
>
> StringBuffer : 연산이 많고 스레드 (동기화) 가능 
>
> StringBuilder : 연산이 많고 동기화를 고려하지 않음



## String

```java
package basic02_api;

import java.util.Arrays;

public class StringTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		String name = "John";
		String name2 = "John";
		
		String addr = new String("570 21st Street, Oakland, CA 94612");
		String addr2 = new String("570 21st Street, OAKLAND, CA 94612"); // 영어대소문자 구분
		
		// == : 
		if(name == name2) {
			System.out.println("name and name 2 are same");
		} else {
			System.out.println("name and name 2 are different");
		}
		
		// == : 
		// 이 경우는 다르다. 왜냐? new String 으로 객체를 만들어서 call by reference 
		// 즉 다른 주소를 비교하는것 
		if(addr == addr2) {
			System.out.println("addr and addr 2 are same");
		} else {
			System.out.println("addr and addr 2 are different");
		}
		
		// 
		String addr3 = addr2;
		if(addr2 == addr3) {
			System.out.println("addr 2 and addr 3 are same");		
		} else {
			System.out.println("addr 2 and addr 3 are same");
		}
		
		// 그렇다면 객체의 주소 안을 비교하려면?
		// equals() 메소드 사용 : 객체내의 값을 비교해준다. (대소문자 구별)
		boolean boo = addr.equals(addr2); // addr2.equals(addr) 똑같은 말
		if(boo) {
			System.out.println("addr and addr 2 are same");
		} else {
			System.out.println("addr and addr 2 are different");
		}
		
		// 만약 String 객체안에 대소문자 구분없이 비교를 하고 싶다면? 
		// equalsIgnoreCase() : 값을 비교해준다. (대소문자 구별하지 않는다.)
		if(addr.equalsIgnoreCase(addr2)) {
			System.out.println("Same. Igonore case-sensitivity");
		} else {
			System.out.println("Different. Igonore case-sensitivity");
		}
		
		String str = "A";
		str = str + "B";
		String str2 = str + 100;
		
		System.out.println("charAt()->"+addr.charAt(7)); // index위치의 문자를 반환한다.
		// concat 은 문자연결 
		System.out.println("concat()->"+addr.concat(name)); // 문자연결
		
		// addr = addr.concat(name);
		// System.out.println("concat()->"+addr);
		
		// ------------------------------------------------------------------
		// 카톡에서 hello 라고 보내면 코드로 전환되서 보내진다. 
		String txt = "Hello!";
		byte txtCode[] = txt.getBytes();
		System.out.println(Arrays.toString(txtCode));

		// ------------------------------------------------------------------
		int idx = addr.indexOf("e"); // 앞에서부터 
		System.out.println("indexOf->" + idx);
		
		int lastIdx = addr.lastIndexOf("e"); // 뒤에서부터 
		System.out.println("indexOf->" + lastIdx);
		
		System.out.println("indexOf->" + addr.indexOf("e", 5)); // 몇번쨰부터 찾아줘
		
		// 글자 길이
		System.out.println("length->" + addr.length());
		
		// 반복 
		System.out.println("repeat->" + addr.repeat(3));
		
		// 반복을 이용해서 선긋기 
		System.out.println("*".repeat(50));
		
		// 다른 문자로 바꾸기 
		addr = addr.replaceAll("94612", "Zip Code");
		System.out.println("replaceAll->"+ addr);
		
		// split : 쪼개기 
		String tel = "010-9234-0392";
		String telSplit[] = tel.split("-");
		System.out.println(Arrays.toString(telSplit));
		
		// substring : 문자열에서 일부의 문자열을 얻을 떄 
		String sub1 = addr.substring(6, 15); // 6번째부터 14번까지
		System.out.println("substring(int, int)->" + sub1);
		String sub2 = addr.substring(15); // 14번째부터 끝까지 
		System.out.println("substring(in)->" + sub2);
		
		// 대소문자 변환
		System.out.println("lower->" + addr.toLowerCase());
		System.out.println("upper->" + addr.toUpperCase());
		
		// ------------------------------------------------------------------
		// 문자열로 받았기때문에 계산이 되지 않는다. 
		String x = String.valueOf(5.3) + 500;
		System.out.println(x);
		
		char c[] = {'J', 'a', 'v', 'a'}; // Java 로 붙여서 출력하고 싶다면?
		System.out.println(c);
		System.out.println(String.valueOf(c));
		
		// trim : 공백 
		String data = "			Test    Programming			";
		System.out.println("data->"+ data.trim()+"?"); // 양쪽 끝의 공백문자를 지운다.
	}

}

```



## StringBuffer

```java
package basic02_api;

public class StringBufferTest {

	public static void main(String[] args) {
		// String : 연산이 적은 데이터 스레드 가능
		// StringBuffer : 연산이 많고 스레드 (동기화) 가능 
		// StringBuilder : 연산이 많고 동기화를 고려하지 않을
		
		// StringBuffer sb = new StringBuffer(30); // 메모리 미리 확보 가능
		StringBuffer sb = new StringBuffer("JAVA Programming Test.........");
		// 확보한 메모리 확인 
		System.out.println("capacity->" + sb.capacity());
		
		
		// 마지막에 문자 추가
		sb.append("잘되나?"); 
		// 4번째에 문자 추가
		sb.insert(4, "(자바)");  
		System.out.println(sb);
		// 거꾸로 뒤집기
		sb.reverse(); 
		System.out.println(sb);
		// 문자열 지우기 
		sb.delete(5, 10); 
		System.out.println(sb);
		
		
		// equals() ;
		// compareTo() : 문자와 문자를 비교하여 정수를 리턴해준다.
		StringBuffer first = new StringBuffer("Java");
		StringBuffer second = new StringBuffer("JAVA");

		int result = first.compareTo(second);
		int result2 = second.compareTo(first);
		System.out.println("result->" + result + ", result2->" + result2);
		
		// 0 : 두 객체의 데이터는 같다.
		// 양수 : 왼쪽객체의 데이터가 크다.
		// 음수 : 왼쪽객체의 데이터가 작다.
		


	}

}

```



## StringTokenizer

```java
package basic02_api;

import java.util.StringTokenizer;

public class StringTokenizerTest {

	public static void main(String[] args) {
		// StringTokenizer : 문자열 조각내기
		String colorName = "Red,Blue,Green,Cyan,..Yellow/Brown";
		
		StringTokenizer st = new StringTokenizer(colorName, ",./");
		
		// 조각낸 문자 세기 
		System.out.println("tokencounts->" + st.countTokens());
		
		while(st.hasMoreElements()) { // true: 토큰이 있다.  false: 토큰이 없다. 
			System.out.println(st.nextElement());
		}

	}

}

```

