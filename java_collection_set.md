# Java Collection Framework - Set 

> Thu Jun 16, 2022

---

[toc]

> Set 는 List 와 다르게 객체 (데이터) 를 중복해서 저장할 수 없습니다. 또한 저장 순서가 보장되지 않습니다.



Set 컬렉션을 구현하는 클래스들이 공통적으로 사용하는 주요 메소드는 다음과 같습니다. [SetAPI](https://docs.oracle.com/javase/7/docs/api/)

![image-20220616152316740](java_collection_set.assets/image-20220616152316740.png)



### HashSet

데이터를 중복 저장할 수 없고 순서를 보장하지 않으며 다음과 같이 생성합니다.

`Set<E> 객체명 = new HashSet<E>`

```java
import java.util.HashSet;
import java.util.Iterator;

public class HashSetTest {

	public HashSetTest() {
		
	}
	public void start() {
		// Set : 입력순서를 유지하지 않는다. 
		//		 객체의 중복을 허용하지 않는다.
		
		int[] data = {25,54,35,24,24,34,56,75,75,87,87};
		
		HashSet<Integer> hs = new HashSet<Integer>();
		for(int n: data) {
			hs.add(n);
		}
		
		// HashSet 의 객체 얻어오기 
		Iterator<Integer> i = hs.iterator();
		while(i.hasNext()) { // 있으면 true, 없으면 false
			int d = i.next();
			System.out.println(d);
			
		}
	}

	public static void main(String[] args) {
		new HashSetTest().start();

	}

}
```



### TreeSet

TreeSet도 HashSet과 같이 중복된 데이터를 저장할 수 없고 입력한 순서대로 값을 저장하지 않습니다. 차이점은 TreeSet은 기본적으로 **오름차순**으로 데이터를 **정렬**합니다.

`Set<Integer> set = new TreeSet<Integer>();`

```java
import java.util.Iterator;
import java.util.TreeSet;

public class TreeSetTest {

	public TreeSetTest() {
		
	}
	public void start() {
		// TreeSet: 입력순서를 유지하지 않는다.
		//			중복데이터 허용하지 않음
		//			객체를 오름차순으로 정렬한다. 
		TreeSet<Integer> ts = new TreeSet<Integer>();
		
		int[] data = {52,9,47,63,67,87,23,52,9,47,63,67};
		
		for(int d: data) {
			ts.add(d);
		}
		
		Iterator<Integer> ii = ts.iterator();
		while(ii.hasNext()) {
			System.out.println(ii.next());
		}
		
	}

	public static void main(String[] args) {
		new TreeSetTest().start();

	}

}

```

