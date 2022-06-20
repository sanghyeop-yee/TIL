# Java Thread

> Mon Jun 20, 2022

---

프로그램을 여러개를 실행하는것은 운영체제의 시분할을 통해서 가능합니다.

스레드는 한 프로세스 안에서 여러개의 프로그램이 돌아가는것처럼 가능하게 합니다. 



Thread 클래스를 상속받고 스레드 처리가 필요한 코드를 run()에 오버라이딩하면

자바 가상머신의 스레드 스케쥴러가 실행을 관리합니다.

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

