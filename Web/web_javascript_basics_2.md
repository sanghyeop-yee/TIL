# JavaScript

> Mon Oct 31, 2022

---

자바스크립트는 좀 널널한 문법을 가지지만 조금 특이합니다.

### 변수

> var - 변경할 수 있는 변수 선언
>
> const - 바꿀 수 없는 변수 
>
> let - 일반적인 방법



### 함수

함수 = 마법 = 코드 묶음

Semicolon (;) 은 써줘도 되고 안써줘도 되지만 통상 써줍니다.

``` javascript
function 함수이름(매개변수){
		공식
    리턴
}

// 정의
function myFunction(x){
  let temp = 2*x + 3
  return temp
}

// 실행
myFunction(1)
```



입력, 출력이 없을 수 있습니다. 

```javascript
function fly(){
	openWings();
  while(true){
    moveWings();
  }
}
```



다른 방법으로도 함수를 정의 할 수 있습니다.

```javascript
// 방법 1
function add(x, y){
  let temp = x + y;
  return temp;
}

// 방법 2
add = (x, y) => {
  let temp = x + y;
  return temp;
}
```





### 반복문

나무를 10번 찍는다.

```javascript
for(let i=0; i<10; i++){
  console.log("나무 찍기" + i);
}
```



혹은 배열에서 하나씩 값을 이용한 반복문을 사용한다면.

```javascript
myArray = [1,2,3,4,5]

myArray.forEach(e => { // 여기서 e는 element. i 나 다른 것으로 대체 가능합니다.
  console.log("나무 찍기 " + e);
});
```





### BOM

> BOM : Browser Object Model 
>
> ​	뒤로 가기, 앞으로 가기, 새로고침, 창 띄우기 등 브라우저의 기능을 조작 가능 



```javascript
alert(); 

history.back();
```



### DOM

> DOM : Document Object Model
>
> ​	태그를 추가하거나 삭제하거나 클릭했을때 바꿔라 등의 HTML 을 조작 가능. 

#### 웹 조작 순서

1. 대상 선택
2. 조작



#### 대상 선택 방법

1. getElementBy~()
2. getElementsBy~()
3. querySelector()



#### 조작 방법

1. .innerHTML = 
2. .style.color =
3. ...



getElementBy~()

```javascript
// 1. 대상 선택
let elm = document.getElementById("johnjohn")
// 2. 조작 
elm.innerHTML = "이마지"
elm.style.color = "red"
```

getElementsBy~()

```javascript
document.getElementsByClassName("gb_d")

document.getElementsByClassName("gb_d")[0]

document.getElementsByClassName("gb_d")[1]
```

querySelector()

```javascript
document.querySelector(".MV3Tnb")

document.querySelector(".MV3Tnb").innerHTML = "안뇽"
```

querySelectorAll()

```javascript
document.querySelectorAll(".MV3Tnb")

document.querySelectorAll(".MV3Tnb")[0].innerHTML = "안뇽"
```



### 이벤트

> 클릭, 스크롤, 키보드 입력, 로딩 등... => 행동!
>
> 이벤트 발생하면 뭐할지 정해줄 수 있습니다.



#### 이벤트 등록 방법

1. inline
2. addEventListener



inline 방법

```javascript
<img class="img1" onClick="alert('클릭되었습니다!')">
```



addEventListener 방법

```javascript
<img id="googleLogo"></img>

document.getElementById("googleLogo").addEventListener('click',function(event){
  alert("구글로고를 클릭하셨군요!");
})
  
```

