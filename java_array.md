# Java Array

> Fri Jun 10, 2022

___

1차원 배열의 초기값 설정하기

`int arr[] = {56, 84, 61, 15, 3, 78, 95, 42}`



## 버블 정렬 (BubbleSort)

> 정렬 API 를 사용하지 않은 가장 기본적인 정렬 방법

```java
public class ArrayBubbleSort {

	public static void main(String[] args) {
		// 배열의 값을 크기순으로 정렬하기
		int arr[] = {56,84,61,15,3,78,95,42};
		
		// 1. 정렬전
		System.out.print("정렬전 ->");
		for(int i=0; i<arr.length; i++) { // 0,1,2,3,4,5,6,7
			System.out.print(arr[i]+"\t");
		}
		System.out.println();
		
		// 3. 반복해서 정렬
		for(int j=0; j<arr.length-1; j++) { // 0,1,2,3,4,5,6

			// 2. 한번 정렬             8-1-0   8-1-1   8-1-2
			for(int i=0; i<arr.length-1; i++) { // 0,1,2,3,4,5,6
				// i 가 0인 상태로 들어와서 0번쨰 값과 1번째값과 비교하여 0값이 더 크면 교환한다
				// 배열의 i 번째와 i+1 번쨰 꺼랑 비교!
				// 비교했을때 왼쪽이 더 크면 교환!
				if(arr[i] > arr[i+1]) { 
					int temp = arr[i]; // 변수를 하나 만들어서 왼쪽의 큰 애를 보관
					arr[i] = arr[i+1]; // 왼쪽값은 이제 오른쪽값으로 교환하고
					arr[i+1] = temp; // 오른쪽 값은 보관된 왼쪽 변수값으로 교환!
				}
				
			}
			// 3. 정렬후 확인하며 출력
		System.out.print("정렬후 ->");
		for(int i=0; i<arr.length; i++) {
			System.out.print(arr[i]+"\t");	
		}
		
		System.out.println();
		}
	
	}

}
```



## API 사용해서 정렬하기

```java
package basic02_api;

import java.util.Arrays;
import java.util.Collections;

public class ArraysTest {

	public static void main(String[] args) {
		// Arrays 클래스: 배열을 이용해 처리한다 
		
		// 1차원 배열 선언
		int data[] = {85,23,53,64,34,62,56};
		
		// Java.util 안에 있는 toString 메소드로 하면 문자열이 하나 돌아온다.
		// toString() : 배열의 값을 문자로 만들어 리턴해준다. 
		// String lst = Arrays.toString(data); // [85,23, ... ] 
		System.out.println("정렬전 = "+ Arrays.toString(data));
		
		// 오름차순으로 정렬 
		Arrays.sort(data); 
		System.out.println("오름차순 정렬후 = "+ Arrays.toString(data));
		// Arrays.sort(data,2,6); // 3번쨰부터 6번째까지
		
		// 내림차순
		Integer data2[] = {85,23,53,64,34,62,56}; // Integer 라는 클래스형으로 
		Arrays.sort(data2, Collections.reverseOrder());
		System.out.println("내림차순 정렬후 = "+ Arrays.toString(data2));
		
		// 배열의 복사 
		int data3[] = {85,23,53,64,34,62,56};
		int copy[] = Arrays.copyOfRange(data3, 2, 6);
		System.out.println("copy="+ Arrays.toString(copy));
		
		// 두개의 배열의 일치를 확인할
		int data4[] = {85,23,53,64,77,62,56};
		System.out.println(Arrays.equals(data, data4));

	}

}

```

