# Java Collection Framework - List

> Thu Jun 16, 2022

---

[toc] 

## Collection Framework (자료구조)

> 컬렉션이란 쉽게말해 데이터를 담을수 있는 주머니로, 객체나 데이터들을 효율적으로 관리(추가, 삭제, 검색, 저장)하기 위해서 사용하는 라이브러리를 의미합니다.



배열은 저장할 크기가 배열을 생성할 때 결정되어있어 배열의 크기가 넘어가면 저장이 불가능하죠. 
또한 데이터를 삭제하면 해당 인덱스의 데이터는 비워있는 구조를 갖는 등 여러 문제점이 발생됩니다.



이러한 문제점을 컬렉션 프레임워크를 통해서 해결이 가능합니다. 
java.util 패키지에 Collection 과 Map 인터페이스가 있습니다.



- Collection 인터페이스 상속받는 대표적인 인터페이스 List, Set
  - List 을 구현하는 클래스들: ArrayList, Vector, LinkedList
  - Set 을 구현하는 클래스들: HashSet, LinkedHashSet, TreeSet
- Map 인터페이스를 상속받는 구현 클래스 HashMap, Hashtable, TreeMap



![image-20220616132618417](java_collection_framework.assets/image-20220616132618417.png)

![image-20220616195247399](java_collection_framework.assets/image-20220616195247399.png)

## List

> 인덱스 순서로 저장이 되며, 중복된 데이터 저장이 가능하고 데이터를 일렬로 늘어놓는 구조입니다.

![image-20220616195652024](java_collection_framework.assets/image-20220616195652024.png)

### ArrayList

아래는 ArrayList 를 생성하는 방법입니다.

`List<E> 객체명 = new ArrayList<E>([초기 저장용량]);`

* <E> 는 제네릭 타입이며 생략시 Object 타입
* Object 은 모든 데이터 타입 저장 가능. 데이터 추가 및 검색시 형변환 필요.



### Generic

> 하나의 데이터 타입만 받을수 있다고 미리 선언하여 형변환없이 사용 가능

K -> Key

V -> Value

E -> Element (매개변수)



### LinkedList

FIFO (First-In First-Out) 은행 번호표같은 Queue 기능은 LinkedList 에 존재합니다.

양쪽에서 꺼내고 집어넣을수 있는 Deque 기능 역시 LinkedList 에 존재합니다.



## Stack

```java
import java.util.Stack;

public class StackTest {

	public StackTest() {
		// Stack : 한쪽으로 객체를 담거나 꺼낸다. (FILO, LIFO)
		//		   객체를 꺼내면 Stack 영역에 지워진다. 
		
		Stack<String> s = new Stack<String>();
		s.push("John");
		s.push("MJ");
		s.push("Andrew");
		
		System.out.println("empty()->"+ s.empty()); // 객체가 있으면 False, 없으면 True
		System.out.println("size()->"+ s.size()); 
		
		while(!s.empty()) { // False
			System.out.println(s.pop());
			System.out.println("남은객체수->"+ s.size());
		}
		/// True

	}

	public static void main(String[] args) {
		
		new StackTest();

	}

}
```



