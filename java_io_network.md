# Java Network

> Mon Jun 20, 2022

---

> TCP - 전화처럼 연결
>
> UDP - TV, 라디오처럼 한쪽에서만 송출

통신을 위해서는 IP 가 있어야 하며 IP 하나에 65535 개의 Port 가 있습니다. 



* Terminal 에서 IP Address 확인하기

`ipconfig getifaddr en0`



`java.net.ServerSocket` : Client 가 접속하기를 기다리는 클래스

서버에서 `ServerSocker` 객체를 만들어놔야 합니다.

Socket 이 접속을 하는데 IP Address 와 Port 번호가 있어야 합니다.



### InetAddress

InetAddress 클래스를 사용해서 IP Address 를 객체로 만들어야합니다.

```java
import java.net.InetAddress;

public class InetAddressTest {

	public InetAddressTest() {
		
	}
	public void start() {
		try {
			// ip 에 관련된 객체를 생성한다. 
			// 내컴퓨터의 아이피정보 얻어오기.
			InetAddress ia = InetAddress.getLocalHost();
			String ip = ia.getHostAddress(); // 내컴퓨터 ip
			String name = ia.getHostName(); // 컴퓨터이름 또는 url 주소
			System.out.println("ia.address->"+ip);
			System.out.println("ia.name->"+name);
			
			// 도메인 이용한 InetAddress 를 얻어오기
			InetAddress ia2 = InetAddress.getByName("www.google.com");
			System.out.println("ia2.address->"+ia2.getHostAddress());
			System.out.println("ia2.name->"+ia2.getHostName());
			
			// 아이피를 이용한 InetAddress 를 얻어오기
			InetAddress ia3 = InetAddress.getByName("223.130.195.200");
			System.out.println("ia3.address->"+ia3.getHostAddress());
			System.out.println("ia3.name->"+ia3.getHostName());
			
			System.out.println("---------------------------------------------");
			
			// 여러개의 InetAddress 객체 얻어오기 
			InetAddress[] ia4 = InetAddress.getAllByName("www.google.com");
			for(InetAddress i : ia4) {
				System.out.println("ia4.address->"+i.getHostAddress());
				System.out.println("ia4.name->"+i.getHostName());
			}
			
		}catch(Exception e) {
			e.printStackTrace();
		}
		
	}

	public static void main(String[] args) {
		InetAddressTest it = new InetAddressTest();
		it.start();

	}

}
```



### URL



```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.URL;
import java.net.URLConnection;

public class URLTest {

	public URLTest() {
		
	}
	public void start() {
		try {
			URL url = new URL("https://www.seoul.go.kr/main/index.jsp");
			System.out.println("protocol->"+ url.getProtocol());
			System.out.println("port->"+ url.getPort());
			System.out.println("host->"+ url.getHost());
			System.out.println("path->"+ url.getPath());
			System.out.println("file->"+ url.getFile());
			//-------인코딩 코드-------------------------------------------
			// URL Connection 객체를 통해 header 정보를 얻어올수 있다.
			URLConnection connection = url.openConnection();
			connection.connect(); // 채널확보하여 헤더정보 가져오기
			String header = connection.getContentType();
			// System.out.println(header);
			String encode = header.substring(header.indexOf("=")+1); // UTF-8, euc-kr
			
			//----------------------------------------------------------
			
			InputStream is = url.openStream(); //byte 단위
			InputStreamReader isr = new InputStreamReader(is, encode); //문자 단위
			BufferedReader br = new BufferedReader(isr); // 한줄 단위 
			
			File f = new File("/Users/myname/testFolder/seoul.html");
			FileWriter fos = new FileWriter(f);
			BufferedWriter bw = new BufferedWriter(fos);
			
			String inData="";
			while((inData = br.readLine()) != null) {
				System.out.println(inData);
				bw.write(inData+"\n");
			}
			
			
		}catch(Exception e) {
			e.printStackTrace();
		}
	}

	public static void main(String[] args) {
		URLTest ut = new URLTest();
		ut.start();

	}

}
```

