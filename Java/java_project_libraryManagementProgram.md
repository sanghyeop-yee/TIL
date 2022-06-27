# Library Management Program

> Thu Jun 23, 2022

---

![image-20220623092154016](/Users/sanghyeop/Library/Application Support/typora-user-images/image-20220623092154016.png)

1. Menu [1.Book List, 2.Add, 3.Edit, 4.Delete, 5.End, 6.Search]
2. Book [ID, Title, Author, Publisher, Category, Price]
3. Exit => Save
4. HashMap을 사용하여 Key 와 Values 로 구성된 객체를 저장
   1. Key 중복없이 저장하기
   2. Key 로 오름차순 정렬하기



## BookStart.java

### Book List

리스트를 실행했을때 컬럼 헤더를 표시할 수 있는 headerPrint() 메소드와 데이터를 출력할 수 있는 bookList() 메소드를 만듭니다.



### Book Add

입력받은 책 정보를 저장할 vo 객체를 생성하고 각 항목의 값을 입력받습니다.

책 한권의 정보가 모두 입력되면 컬렉션에 키와 값을 구분하여 vo 객체를 추가합니다. 



## BookVO.java

직렬화가 되어 있어야 파일에 읽고 쓸 수 있는 클래스가 됩니다.

`public class BookVO implements Serializable`



Extends 는 일반 클래스와 abstract 클래스 상속에 사용되고 
Implement 는 interface 상속에 사용됩니다.



상속은 extends 키워드를 사용해서 부모 클래스의 메소드 및 필드를 물려받습니다. 만약 자식 클래스에서 부모 클래스로부터 상속받은 메소드를 다르게 정의할 필요가 있다면 메소드 오버라이딩을 사용할 수 있습니다.



## BookDataSet.java

