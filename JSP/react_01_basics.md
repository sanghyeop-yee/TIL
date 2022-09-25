

# React 01 : Basics

> Wed Sep 14, 2022

---

[toc]

Nodes, vuejs, reactjs, angularjs... 등 Javascript 로 만들어져있는 라이브러리 입니다. 

사용방법은 CDN, 함수, 클래스 방식이 있습니다. 주로 함수를 사용하기 때문에 이 방식을 배워봅시다.

React 는 프론트 전용 라이브러리 입니다.



CDN 방식으로 가져오기 위해서는 아래의 링크 세줄이 필요합니다.

reactjs.org > get started > cdn links 의 순서로 가서 링크를 가져옵니다. 

```html
<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
```



Babel은 자바스크립트 컴파일러입니다. 

 https://babeljs.io > 상단의 docs > 왼쪽사이드 메뉴에서 @babel/standalone 로 가서 아래의 링크를 가져옵니다.

```html
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
```



함수를 이용하여 내용을 만들때

태그를 두개를 사용하려면 하나의 태그 (div) 안에 포함되어야 합니다.

```html
<script type="text/babel">
        // 함수를 이용하여 내용을 만들기
        function StartReact(){
            return (<div><h1>리엑트를 CDN으로 작성하기</h1><h2>두번째 태그</h2></div>);
        }
    </script>
```



완성된 코드는 다음과 같습니다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- reactjs.org > get started > cdn links -->
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <!-- Babel은 자바스크립트 컴파일러이다.
        https://babeljs.io > 상단의 docs > 왼쪽사이드 메뉴에서 @babel/standalone
    -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body>
    <div id="myDiv"></div>
    <script type="text/babel">
        // 함수를 이용하여 내용을 만들기
        function StartReact(){
            return (<div>
                        <h1>리엑트를 CDN으로 작성하기</h1>
                        <h2>두번째 태그</h2>
                    </div>);
        }

        // 리엑트 처리하기 - DOM에 함수에서 리턴하는 컨텐츠를 표시하기
        //            호출할 함수, return결과가 포시될 곳
        ReactDOM.render(<StartReact />, document.getElementById("myDiv"));
    </script>
    
</body>
</html>
```





## React Class Constructor

자바스크립트로 클래스를 만드는 방법은 아래와 같습니다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>
        <script>
            // 클래스
            class Car{
                constructor(name){
                    this.brand = name;    // 전역변수에 name의 값을 대입시킴
                }
            }

            const myCar = new Car("롤스로이스");
            document.write("브랜드: ", myCar.brand)
        </script>
    </h1>
    
</body>
</html>
```





## React Class Method

자바스크립트로 클래스 메소드를 만드는 방법은 아래와 같습니다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script>
        class Car{
            constructor(carName){
                this.brand = carName;
            }
            // 메소드 생성
            present(){
                return '나는 '+ this.brand+ '를 2023년에 산다.';
            }
        }

        const myCar = new Car("Tesla X");
        // 객체내의 메소드 호출
        document.write(myCar.present());
    </script>
    
</body>
</html>
```





## React Class Inheritance

자바스크립트로 클래스 상속을 만드는 방법은 아래와 같습니다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script>
        class Car{
            constructor(carName){ // 3-2.
                // 전역변수
                this.brand = carName;
            }
            present(){
                return '나는 ' + this.brand + '산다';

            }
        }

        class Model extends Car{
            constructor(carName, carModel){ // 2.  // 4-1
                // 상위클래스의 생성자 호출하여 실행하기
                super(carName); // 3-1. 
                this.model = carModel; // 4-2.
            }
            show(){
                return this.present() +', 모델은 ' + this.model; // 5.
                return msg;
            }
        }

        const model = new Model('롤스로이스', '컬리넌'); // 1. 객체생성 
        document.write(model.show()); // 6.
    </script>
    
</body>
</html>
```





## React 개발환경 및 프로젝트 생성

사용할 폴더로 이동하여 터미널에서 리엑트를 설치합니다.

```sh
npm install -g create-react-app

// 맥에서
npx create-react-app react00_start
```



설치가 완료됐으면 시작합니다.



포트번호를 바꾸려면 package.json 을 열어서 "scripts" 부분에 아래와 같이 추가해줍니다.

```json
 "scripts": {
    "start": "export PORT=3001 && react-scripts start",
		...
  },
```







## JSX

> JSX 는 JavaScript 의 syntax 확장입니다. 사용하는 것을 권장합니다. 
