# React 02 : Components

> Fri Sep 16, 2022

---

[toc]

부트스트랩 다운 받기

`cd react02_components`

`npm install react-bootstrap bootstrap`



### App04.js

```js
import React from 'react'
import Table from 'react-bootstrap/Table'
import MyComp from './example/ex04.js'

class App extends React.Component{
    render(){
        return(
            <div className='container'>
                <MyComp></MyComp>
                <hr/>
                <Table className='table-dark'>
                    <tr><td>111</td><td>2222</td></tr>
                    <tr><td>333</td><td>4444</td></tr>
                </Table>
            </div>
        )
    }
}

export default App;
```



### ex04.js

```js
import React from "react";
class MyComp extends React.Component{
    bgcolor = 'red';

    state = {
        mycolor : 'yellow',
        flag : false
    }
    // 컬러를 변경하는 함수
    bgChangeEvent = ()=>{
        console.log(this);
        this.bgColor = 'green';
        // bgcolor 가 변경되더라도 자동적용되지 않는다.
        console.log(this.bgcolor);
    }
    bgChangeEvent2(){
        // state에 키가 true면 mycolor를 lightblue, false면 mycolor를 orange로 변경
        // flag는 부정한다.

        let changecolor = (this.state.flag)? 'lightblue': 'orange';
        this.setState({mycolor:changecolor, flag:!this.state.flag});
    }
    render(){
        return(
            <div className="container">
                <h1>변수 (일반변수, state변수)</h1>
                <button style={{backgroundColor:this.bgcolor}}
                    onClick={this.bgChangeEvent}
                >클릭하면 버튼의 배경색이 바뀌어요.(안됨)</button><hr/>
                <button style={{backgroundColor:this.state.mycolor}}
                    onClick={this.bgChangeEvent2.bind(this)}
                    >클릭하면 색상이 변경되요.</button>
            </div>
        )
    }
}

export default MyComp;
```



### index.js

```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
// import App from './App';
import reportWebVitals from './reportWebVitals';

import App from './App04';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();

```







### App05.js

```js
import React from "react";
import MyComp from './example/ex05';
export default class App extends React.Component{

    render(){
        return(
            <div className="container">
                <MyComp></MyComp>
            </div>
        )
    }
}
```



### ex05.js

```js
import React, {Component} from 'react';
class MyComp extends Component{
    state = {
        items:[
            '오늘은 즐거운 금요일이네요',
            '내일은 뭘할까?',
            '즐거운 하루 보내세요.',
            'React 까다롭네...',
        ],
        title : 'React Test...'
    }

    render(){
        // 객체 비구조화 할당
        const {title, items}= this.state;

        // 1. for문 이용하여 태그만들기
        let str1=[];
        for(let i=0; i<items.length; i++){
            str1.push(<li key={i} style={{color:'green'}}>{items[i]}</li>);
        }
        // 2. map을 이용하여 태그만들기
        //    배열명.map((obj, idx)=>{});
        //    str2에 리턴한 li태그가 누적된다.
        let str2 = items.map((obj, idx)=>{
            return <li key={idx} style={{color:'orange'}}>{obj}</li>
        });

        return (
            <div className="container">
                <h1>{title}</h1>
                <ol>{str1}</ol>
                <ul>{str2}</ul>
            </div>
        )
    }
}
export default MyComp;
```



