# Java Review

> Fri Jul 1, 2022

---



[toc]

## [형변환]

### String --> int (문자열을 숫자로)

* Integer.parseInt()

```java
String str1 = "123";
int intValue1 = Integer.parseInt(str1);
```



### int --> String (숫자를 문자열로)

* Integer.toString()

```java
int intValue1 = 123;
String str1 = Integer.toString(intValue1);
```

* String.valueOf()

```java
int intvalue1 = 123;
String str1 = String.valueOf(intValue1);
```



### double --> int

```java
double a = 253.635

int b = (int)a;
int c = (int)Math.ceil(a);
```



## [접근제한자]

### private > default > protected > public

* public: 공용, 누구나
* protected: 같은 패키지, 다른 패키지인 경우 상속받음
* default: 같은 패키지
* private: 같은 클래스에서 상속안함, 오버라이딩안됨



## [컬렉션]

### List, Set, Map

* List: 순서유지, index 있음, 중복허용
  * ArrayList, LinkedList, Vector
* Set: 입력순서를 유지하지 않는다. 중복 허용안함
  * HashSet (정렬안됨), TreeSet (정렬됨)
* Map: key 와 value, key 중복 허용안함
  * HashMap, Hashtable (동기화), TreeMap (key로 정렬)



## [오버라이딩, 오버로딩]

### 오버라이딩

* 상속관계에서 상위클래스의 메소드를 재정의함
* 메소드명, 매개변수 갯수, 매개변수 타입이 같아야함

### 오버로딩

* 같은 클래스에서 메소드명이 같은 메소드를 여러개 생성하는 것을 말함
* 단, 매개변수의 갯수나 데이터형이 달라야함

```java
method1(int a){
}
method1(String b){
}
method1(int a, String b){
}
```



## [상속]

### 추상클래스

* 일반메소드와 추상메소드가 존재함
* extends 로 상속받아 모든 추상메소드에 오버라이딩 해야함



### 인터페이스

* 추상메소드, final static 변수가 존재하는 클래스
* Implements 로 상속받아 추상메소드 모두 오버라이딩 해야함
* 인터페이스가 인터페이스를 상속받을 경우 extends 로 상속받음



## [입출력]

### byte 단위

* InputStream, OutputStream

### char (문자) 단위

* InputStreamReader, OutputStream

### 줄 단위

* BufferedInputStream, BufferedOutputStream, BufferedWriter, BufferedReader



## [네트워크]

### TCP : 연결 후 통신

* 전화처럼 연결
* Socket, ServerSocket



### UDP: 비연결상태에서 통신

* TV, 라디오처럼 한쪽에서만 송출
* DatagramSocket, MulticastSocket



# MySQL Review

## [내장함수]

### 숫자

* round(), ceil(), floor()...

### 문자

* concat(), substr(), length(), char_length(), bit_length()...

### 날짜

* now(), curdate(), adddate(), last_day()

### 변환

* date_format(now(), '%Y %m %d')

### 그룹함수

* sum(), avg(), count(), max(), min()...



## [데이터조작어 (DML)]

* select
* insert
* update
* delete





## [상속문제]

[1-1] SubTest 클래스 객체를 `SubTest ob = new SubTest();` 처럼 생성될 때 SuperTest 의 인스턴스 변수 a에 10을 대입하기 위해 다음 프로그램의 // 에 들어갈 코딩 방법 두가지는?

```java
class SuperTest{
	int a;
	SuperTest(int a){
		this.a=a;
	}
}
class SubTest extends SuperTest{
  public SubTest(int a) {
    super(a);
  }
  public SubTest(){
    // answer below
    
    
  }
}
```



[1-2]  SubTest 클래스 객체를 `SubTest ob = new SubTest();` 처럼 생성될 때 SuperTest 의 인스턴스 변수 a에 10을 대입하기 위해 다음 프로그램의 // 에 들어갈 코딩 방법 세가지는?

```java
class SuperTest{
	int a;
	Super(){
		this(10);
	}
	SuperTest(int a){
		this.a=a;
	}
}
class SubTest extends SuperTest{
  public SubTest(int a) {
    super(a);
  }
  public SubTest(){
    // answer below
    
    
  }
}
```











[2] 다음 실행결과는?

```java
class Test{
	public int a=3;
  
	public void print() {
		a+=5;
		System.out.print("f");
	}
}

class Ex extends Test{
  public int a=8;
  
  public void print(){
    this.a+=5;
    System.out.print("b ");
  }
}

public class Sam{
  public static void main(String[] args){
    Test ob = new Ex();
    ob.print();
    System.out.print(ob.a);
  }
}

```



[3] 다음 실행결과는?

```java
class X{
  X(){
    System.out.print(1);
  }
  X(int x) {
    this();
    System.out.print(2);
  }
}

public class Y extends X{
  Y(){
    super(6);
    System.out.print(3);
  }
  Y(int y){
    this();
    System.out.println(4);
  }
  
  public static void main(String[] args){
    new Y(5);
  }
}
```



[4] 다음 실행결과는?

```java
public class A{
  public void test(){
    
  }
  public String test(){
    return "a";
  }
  public double test(int x){
    return 1.0;
  }
  public static void mian(String[] args){
    A t = new A();
    System.out.println(t.test(25));
  }
}
```



[5] 다음 실행결과는?

```java
class Test{
	int x;
  public void setX(int x){
    this.x = x;
  }
  public int getX(){
    return x;
  }
}

class Ex{
  Test y;
  public void setY(Test y){
    this.y = y;
  }
}

public class Sample{
  public static void main(String[] args){
    Ex o = new Ex();
    Test i = new Test();
    int n = 10;
    i.setX(n);
    o.setY(i);
    System.out.println(o.getY().getX());
  }
}
```





1. 4
2. 2
3. 1
4. 2
5. 3 
6. 4
7. 2
8. 1
9. 3
10. 1
11. 2
12. 4
13. 2
14. 1
15. 3
