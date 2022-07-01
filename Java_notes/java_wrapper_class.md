# Wrapper(포장) Class

> Wed Jun 15, 2022

---

> 기본 타입 (byte, char, short, int, long, float, double, boolean) 의 값을 갖는 객체를 생성할 수 있고 이를 포장 객체라고 합니다. 포장하고 있는 기본 타입 값은 외부에서 변경할 수 없습니다. 만약 내부의 값을 변경하고 싶다면 새로운 포장 객체를 만들어야 합니다.



```java
	public WrapperClass() {
		int i = 500;
		
		// i 를 객체로 만들기
		Integer intObj = Integer.valueOf(i); // new Integer(i);
    // 기본데이터형이 객체화됨 (오토박싱) - 1.5 버전 이상부터 가능
    Integer intObj2 = i;
    
   String data = "254";
		// 문자열을 숫자로 변경
		Integer y = new Integer(data);
		int z1 = Integer.parseInt(data); // int
		Integer z2 = Integer.valueOf(data); // Integer
	}
```



