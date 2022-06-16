# Collection Framework (자료구조)

> Thu Jun 16, 2022

---



> 컬렉션이란 쉽게말해 데이터를 담을수 있는 주머니로, 객체나 데이터들을 효율적으로 관리(추가, 삭제, 검색, 저장)하기 위해서 사용하는 라이브러리를 의미합니다.



배열은 저장할 크기가 배열을 생성할 때 결정되어있어 배열의 크기가 넘어가면 저장이 불가능하죠. 또한 데이터를 삭제하면 해당 인덱스의 데이터는 비워있는 구조를 갖는 등 여러 문제점이 발생됩니다.

이러한 문제점을 컬렉션 프레임워크를 통해서 해결이 가능합니다.

java.util 패키지에 Collection 과 Map 인터페이스가 있습니다.

![image-20220616132618417](java_collection_framework.assets/image-20220616132618417.png)



[java.util.Vector](https://docs.oracle.com/javase/8/docs/api/index.html) 자바 문서에서 벡터를 확인해봅시다.

데이터를 받으면 순서를 유지합니다.



클래스는 다 Object 입니다.

Vector 는 리스트를 상속받고 있습니다.





## Generic

> 하나의 데이터 타입만 받을수 있다고 미리 선언하여 형변환없이 사용 가능

K -> Key

V -> Value

E -> Element (매개변수)





