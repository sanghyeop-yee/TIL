# Java Collection Framework - Map

> Fri Jun 17, 2022

-------



[toc]

## Map Collection

![image-20220617094053991](/Users/sanghyeop/Library/Application Support/typora-user-images/image-20220617094053991.png)

### TreeMap

[Java.util.TreeMap](https://docs.oracle.com/javase/8/docs/api/index.html)

```java
import java.util.Iterator;
import java.util.Set;
import java.util.TreeMap;

public class TreeMapTest {

	public TreeMapTest() {

	}
	public void start() {

		TreeMap<String, MemberVO> tm = new TreeMap<String, MemberVO>();
		
		tm.put("1", new MemberVO(1000, "John", "Business", "010-9324-3242"));
		tm.put("A", new MemberVO(2000, "MJ", "Imagineer", "010-9324-2222"));
		tm.put("Kim", new MemberVO(3000, "Andrew", "HR", "010-9324-3333"));
		tm.put("B", new MemberVO(4000, "Kaycee", "Marketing", "010-9324-4444"));
		tm.put("100", new MemberVO(5000, "Jono", "Design", "010-9324-5555"));
		
		Set<String> keys = tm.keySet();
		Iterator<String> keyList = keys.iterator();
		while(keyList.hasNext()) {
			String key = keyList.next();
			MemberVO vo = tm.get(key);
			System.out.println(key+"->"+vo.toString());
		}	
	}
	public static void main(String[] args) {
		new TreeMapTest().start();
	}
  
}
```



### HashMap

생성하는 방법:

`Map<K, V> map = new HashMap<K,V>();`

`// K 는 키 타입, V 는 값 타입`



## Object Compare

> 사원번호 혹은 사원명을 내림차순과 오름차순으로 정렬하고 싶다면?



사원번호(숫자)를 오름차순으로 정렬하는 내부 클래스 (인터페이스: Comparator)

`class CompareNumAsc implements Comparator<MemberVO>` 로 객체를 만들어서

`Collections.sort` 를 사용할 수 있습니다. 

자바 문서를 확인해봅시다. [java.util.Collections](https://docs.oracle.com/javase/8/docs/api/index.html)



아래 코드는 **사원번호(숫자)** 를 내림차순과 오름차순으로 정렬한 것입니다. 

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class ObjectCompareMain {
	List<MemberVO> list = new ArrayList<MemberVO>();
		
		public ObjectCompareMain() {
			
		}
		public void start() {
			list.add(new MemberVO(3000, "Andrew", "HR", "010-9324-3333"));
			list.add(new MemberVO(1000, "John", "Business", "010-9324-3242"));
			list.add(new MemberVO(5000, "Jono", "Design", "010-9324-5555"));
			list.add(new MemberVO(2000, "MJ", "Imagineer", "010-9324-2222"));
			list.add(new MemberVO(4000, "Kaycee", "Marketing", "010-9324-4444"));
			
			System.out.println("=========정렬전==========");
			for(MemberVO vo:list) {
				System.out.println(vo.toString());
			}
			System.out.println("=========사원번호를 오름차순으로=========");
			Collections.sort(list, new CompareNumAsc());
			
			for(MemberVO vo:list) {
				System.out.println(vo.toString());
			}
			System.out.println("=========사원번호를 내림차순으로=========");
			Collections.sort(list, new CompareNumDesc());
			
			for(MemberVO vo:list) {
				System.out.println(vo.toString());
			}
		
		}
		// 사원번호를 내림차순정렬
		class CompareNumDesc implements Comparator<MemberVO>{

			@Override
			public int compare(MemberVO v1, MemberVO v2) {
				// TODO Auto-generated method stub
				return (v1.getNum()<v2.getNum())? 1 : (v1.getNum()>v2.getNum())? -1 : 0;
			}
			
		}
		

		// 사원번호를 오름차순으로 정렬하는 내부 클래스 (인터페이스: Comparator)
		class CompareNumAsc implements Comparator<MemberVO>{

			@Override
			public int compare(MemberVO v1, MemberVO v2) {
				// 비교를해서 왼쪽이 작거나 같은면 안바꿔도 된다.
				// 왼쪽이 더 크면 바꿔준다. 
				// 0
				// 양수	1200-1000 = 200 
				// 음수	1000-1200 = -200
				//		1200		2000
				return (v1.getNum()<v2.getNum())? -1 : (v1.getNum()>v2.getNum())? 1 : 0 ;
			}
			
		}
	public static void main(String[] args) {
		new ObjectCompareMain().start();

	}

}

```



아래 코드는 **사원명(문자열)** 를 내림차순과 오름차순으로 정렬한 것입니다. 

```java
package basic05_collection;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class ObjectCompareMain {
	List<MemberVO> list = new ArrayList<MemberVO>();
		
		public ObjectCompareMain() {
			
		}
		public void start() {
			list.add(new MemberVO(3000, "Andrew", "HR", "010-9324-3333"));
			list.add(new MemberVO(1000, "John", "Business", "010-9324-3242"));
			list.add(new MemberVO(5000, "Jono", "Design", "010-9324-5555"));
			list.add(new MemberVO(2000, "MJ", "Imagineer", "010-9324-2222"));
			list.add(new MemberVO(4000, "Kaycee", "Marketing", "010-9324-4444"));
			
			System.out.println("=========정렬전==========");
			for(MemberVO vo:list) {
				System.out.println(vo.toString());
			}
      
			System.out.println("=========사원명을 오름차순으로=========");
			Collections.sort(list, new CompareUsernameAsc());
			
			for(MemberVO vo:list) {
				System.out.println(vo.toString());
			}
			System.out.println("=========사원명을 내림차순으로=========");
			Collections.sort(list, new CompareUsernameDesc());
			
			for(MemberVO vo:list) {
				System.out.println(vo.toString());
			}
		
		}
		// 사원명(문자열)을 내림차순으로 정렬
		class CompareUsernameDesc implements Comparator<MemberVO>{

        @Override
        public int compare(MemberVO v1, MemberVO v2) {
          //	   
          return v2.getUsername().compareTo(v1.getUsername());
        }

      }
		
		// 사원명(문자열)을 오름차순으로 정렬
		class CompareUsernameAsc implements Comparator<MemberVO>{

        @Override
        public int compare(MemberVO v1, MemberVO v2) {
          // 0 : 교환안함
          // 1 : 교환
          // - : 교환안함
          return v1.getUsername().compareTo(v2.getUsername());
        }

      }
			
		}
	public static void main(String[] args) {
		new ObjectCompareMain().start();

	}

}


```

