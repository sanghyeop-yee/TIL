# Java Class

> Mon Jun 13, 2022

---

[toc] 

## 객체 지향 프로그래밍

> 부품에 해당하는 객체들을 먼저 만들고, 이것들을 하나씩 조립해서 완성된 프로그램을 만드는 기법 또는 실제 세계를 모델링하여 소프트웨어를 개발하는 방법

* **객체**는 물리적으로 존재하거나 추상적으로 생각할 수 있는 것 중에서 자신의 속성을 가지고 있고 다른 것과 식별 가능한 것을 말합니다.

* 객체는 **상태 (field)**와 **동작 (method)**을 가지고 있습니다.
  * 예) 자동자 객체
    * 상태: 색상, 모델명, 현재 기어, 현재 속도
    * 동작: 기어바꾸기, 감속하기, 가속하기

![image-20220613204857644](java_class.assets/image-20220613204857644.png)

### 객체 지향의 3대 특징

* 캡슐화
* 상속
* 다형성



객체는 간단히 말하면 int a, String a 같은 변수입니다. 단, 객체는 자료형이 클래스 타입인 것이고 일반 자료형과 비교하기 위해 변수라는 말 대신 객체라고 사용합니다.

```java
public class ObjEx01 {
	public static void main(String[] args) {
		String s; // String형 변수 s선언
		s = new String("인스턴스"); // 인스턴스화
		System.out.println(s);
	}
}
```

> 먼저 **String에 s변수(객체)를 선언**하였다. s는 문자열이라는 데이터를 **구현하기 위한 대상**이고 이것이 객체(Object)를 의미한다. 그리고 **s에 "인스턴스"라는 데이터를 new연산자**(인스턴스 생성 역할)**와 String(" ")을 이용하여 전달**(정확히는 인스턴스라는 데이터를 저장하는 heap 메모리의 주소값)하였고 **이러한 과정을 인스턴스화**라고 한다. 이때 **구체적인 실체를 갖게 된 s를 인스턴스**라고 한다.
>
> **[출처]** [[JAVA/자바\] 객체 선언 및 인스턴스 개념](https://blog.naver.com/heartflow89/220952631257)|**작성자** [JOKER](https://blog.naver.com/heartflow89)



* new 는 클래스 타입의 인스턴스 (객체) 를 생성해주는 역할을 담당합니다.
* new 연산자를 통해 메모리 (Heap 영역) 에 데이터를 저장할 공간을 할당받고 그 공간의 참조값을 객체에게 반환하여 주고 이어서 생성자를 호출하게 됩니다.

![image-20220615213031389](java_class.assets/image-20220615213031389.png)

## 클래스

> 클래스 (class): 객체를 만드는 설계도
>
> 인스턴스 (instance): 클래스로부터 만들어지는 각각의 객체를 그 클래스의 인스턴스라고도 합니다.
>
> 하나의 클래스는 필드(Field), 생성자(Constructor), 메소드(Method) 로 구성됩니다.

![image-20220613205018521](java_class.assets/image-20220613205018521.png)

### 클래스의 구조

![image-20220613205055424](java_class.assets/image-20220613205055424.png)

### 객체의 필드와 메소드 사용

* 도트 (.) 연산자 사용

![image-20220613205300743](java_class.assets/image-20220613205300743.png)



## 메소드

> 메소드는 입력을 받아서 처리를 하고 결과를 반환하는 가상적인 상자와 같습니다.



### 메소드의 구조

![image-20220613202412972](java_class.assets/image-20220613202412972.png)

### 메소드의 종료

* return 을 사용합니다.

```java
void myMethod() {
	for(int i=0; i<10; i++){
		if(i == 7)
			return;
	}
}
```

* `return 반환값;` return 뒤에 수식을 적으면 수식의 값이 반환됩니다.



### 인수와 매개 변수

* 메소드 호출시 전달하는 값을 인수 (argument)
* 메소드에서 값을 받을 때 사용하는 변수를 매개 변수 (parameter)

![image-20220613203514312](java_class.assets/image-20220613203514312.png)

### 오버로딩

> 같은 클래스내에서 같은 메소드명이 여러개 존재하는 것을 말합니다.
>
> 매개변수의 갯수나 데이터형이 달라야합니다.

```java

public class Overloading {
	
	public Overloading() {
		
	}
	
	// 오버로딩: 같은 클래스내에서 같은 메소드명이 여러개 존재하는 것을 말한다.
	//		   매개변수의 갯수나 데이터형이 달라야한다. 
	
	// 1~100까지 합
	public void sum() {
		System.out.println("1~100 까지의 합은 5050");
	}
	// 1~?까지의 합
	public void sum(int max) {
		int tot = 0;
		for(int i=1; i<=max; i++) {
			tot += i;
		}
		System.out.println("1~"+max+" 까지의 합은 "+tot);
	}
	// 1~?까지의 홀수 합
	public void sum(int max, String msg) {
		int tot = 0;
		for(int i=1; i<=max; i+=2) {
			tot += i;
		}
		System.out.println("1~"+max+" 까지의 홀수 합은 "+tot);
	}
	// 1~?까지의 짝수 합
	public void sum(String msg, int max) { // 매개변수의 순서가 달라도 가능
		int tot = 0;
		for(int i=2; i<=max; i+=2) {
			tot += i;
		}
		System.out.println("1~"+max+" 까지의 수 합은 "+tot);
	}
	
	
	public static void main(String[] args) {
		// Overloading 클래스를 객체로 만들어서 메소드 사용 
		Overloading o = new Overloading(); // 객체생성
		o.sum(343);
		o.sum(10, "Odd");

	}

}
```



## 상속 (Inheritance)

> 부모 객체는 자기가 가지고 있는 필드와 메소드를 자식 객체에게 물려주어 자식 객체가 사용할 수 있도록 해줍니다.



`public class Sedan extends Car{}`

* 상속관계에서 최상위는 Object 입니다.

