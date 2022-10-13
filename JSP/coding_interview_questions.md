# 코딩 인터뷰 실습

[toc]

## Java

### 1. Scanner 와 BufferedReader 의 차이점?

> BufferedReader 객체를 생성하고 그 안에  InputStreamReader 객체도 생성해주어야 합니다. 
>
> 그리고 BufferedReader 는 readLine() 함수로 받으면 한줄씩 String 형태로 받게 됩니다.  때문에 StringTokenizer 를 생성해서 받아주어야 합니다. 
>
> 그리고 Tokenizer 로 받은 값을 int 형태로 파싱까지 해주어야 합니다. 
>
> 스캐너에 비교하면 꽤 복잡하지만  입력데이터가 많다면 BufferedReader 가 훨씬 빠릅니다.



```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

StringTokenizer st = new StringTokenizer(br.readLine());

int a = Integer.parseInt(st.nextToken());
int b = Integer.parseInt(st.nextToken());
```





### 2. int vs Long

> 코딩테스트에서는 Long 을 사용하면 더 좋습니다.
>
> 자료형의 범위가 더욱 넓기 때문에 가능합니다.
>
> 이상한 음수가 답으로 나온다면 꼭 확인해서 Long 으로 변환해보세요.



```java
int answer = 1000000000;
answer += 2000000000;
System.out.println(answer);
// -1294967296

long answer = 1000000000;
answer += 2000000000;
System.out.println(answer);
// 3000000000 (의도한 답)
```





### 3. ++i vs i++

> 전위연산자 (이기적, 나 먼저) vs 후위연산자 (이타적, 다른 거 먼저)

```java
// 전위연산자
int A[] = new int[3]; // 3칸짜리 배열을 만들고
int i = 1; // i를 1로 초기화 하였습니다. 
A[++i] = 10; // 배열의 인덱스에 전위연산자를 넣고 10을 저장하면?

// 1. i값 1이 2로 증가 (나 먼저)
// 2. A[i]에 10 저장 (i=2)
// A = |0|0|10|

System.out.println(i); // 2
System.out.println(A[1]); // 0
System.out.println(A[2]); // 10
```

```java
// 후위연산자
int A[] = new int[3];
int i = 1;
A[i++] = 10;

// 1. A[i]에 10 저장 (i=1)
// A = |0|10|0|
// 2. i값 2로 증가 (다른 거 먼저)

System.out.println(i); // 2
System.out.println(A[1]); // 10
System.out.println(A[2]); // 0

```





https://www.youtube.com/watch?v=0tnWpmULrdE