# Java Thread

> Mon Jun 20, 2022

---

> 프로그램을 여러개를 실행하는것은 운영체제의 시분할을 통해서 가능합니다. 
>
> 스레드는 한 프로세스 안에서 여러개의 프로그램이 돌아가는것처럼 가능하게 합니다. 

[toc]

Thread 클래스를 상속받고 스레드 처리가 필요한 코드를 run()에 오버라이딩하면

자바 가상머신의 스레드 스케쥴러가 실행을 관리합니다.



### Thread 클래스를 이용한 스레드 구현

```java
public class ThreadTest1 extends Thread{
	String threadName;
	public ThreadTest1() {
		
	}
	public ThreadTest1(String threadName) {
		this.threadName = threadName;
	}

	public void run() {
		for(int i=1; ; i++) {
			System.out.println(threadName + "->" + i);
		}
	}
	public static void main(String[] args) {
		ThreadTest1 t1 = new ThreadTest1("First thread");
		ThreadTest1 t2 = new ThreadTest1("Second thread");
		
		// t1.thread_method();
		// t2.thread_method();

		// run() 메소드는 start() 메소드를 이용하여 호출가능하다. 
		t1.start();
		t2.start();
	}

}
```



### Runnable 인터페이스를 이용한 스레드 구현

```java
public class ThreadTest2 implements Runnable {
	String threadName;
	public ThreadTest2() {
		
	}
	public ThreadTest2(String threadName) {
		this.threadName = threadName;
	}

	public void run() {
		for(int i=1; ; i++) {
			System.out.println(threadName + "->" + i);
		}
	}
	public static void main(String[] args) {
		ThreadTest2 t1 = new ThreadTest2("First thread");
		ThreadTest2 t2 = new ThreadTest2("Second thread");
		
		// 인터페이스를 상속받으면 추상메소드이기 때문에 Thread 객체로 받는다.
		Thread obj1 = new Thread(t1);
		Thread obj2 = new Thread(t2);
		
		obj1.start();
		obj2.start();

	}

}
```



### Thread.sleep 으로 지정한 밀리초만큼 대기

```java
public class ThreadTest2 implements Runnable {
	String threadName;
	public ThreadTest2() {
		
	}
	public ThreadTest2(String threadName) {
		this.threadName = threadName;
	}

	public void run() {
		for(int i=1; ; i++) {
			System.out.println(threadName + "->" + i);
			try {
				Thread.sleep(1000); // 지정한 밀리초만큼 대기한다. 
			}catch(InterruptedException ie) {}
		}
	}
	public static void main(String[] args) {
		ThreadTest2 t1 = new ThreadTest2("First thread");
		ThreadTest2 t2 = new ThreadTest2("Second thread");
		
		// 인터페이스를 상속받으면 추상메소드이기 때문에 Thread 객체로 받는다.
		Thread obj1 = new Thread(t1);
		Thread obj2 = new Thread(t2);
		
		obj1.start();
		obj2.start();

	}

}
```



### Synchronizer (동기화)

만약 하나의 은행계좌에서 엄마와 아들이 인출을 한다고 할때 엄마가 인출하고 있으면 아들은 대기상태 후 인출이 끝나면 아들이 인출할 수 있는 상태를 말합니다.

```java
public class SynchronizerTest {

	public static void main(String[] args) {
		ATM atm = new ATM();
		
		Thread mother = new Thread(atm, "mother");
		Thread son = new Thread(atm, "son");
		
		mother.start();
		son.start();
	}

}

class ATM implements Runnable{
	private int money = 10000;
	
	// 연속으로 7번 인출할 수 있음
	// 스레드 구현할 메소드
	// 스레드 메소드 동기화: synchronized
	// public synchronized void run() {
	public void run() {
		synchronized(this) { // 동기화 시키고 싶은 부분만 할때 
			for(int i=1; i<=7; i++) {
				try {Thread.sleep(1000);}catch(Exception e) {}
				getCash(1000);
			}
		}
	}
	public void getCash(int howMuch) {
		if(money>0) {
			money -= howMuch;
			//												  현재실행중인 스레드 객체 객체의 스레드명
			System.out.printf("%s -> Current balance is %d.\n", Thread.currentThread().getName(), getMoney());
		}else {
			// 스레드 중단 
			try {
				this.wait(); // 현재 실행중인 스레드 중지
			}catch(Exception e) {}
			System.out.println("Balance is not enough.");
		}
	}

	public int getMoney() {
		return money;
	}

	public void setMoney(int money) {
		this.money = money;
	}
	
	
}
```

