---
title: ReactJS - 교육 3일차 - Redux
permalink: 2019-04-02-gasan-class-3day.html
tags: [reactjs]
keywords: reactjs
date: 2019-04-02 00:00:00
summary: React.js 3일차로 배운 내용을 정리한다.
folder: meteopark_programming_reactjs
sidebar: category_sidebar
search: exclude
---


## 네이밍 규칙
- 함수를 만들때는 속성 > 생성자 > 메소드 > 라이프사이클메서드
- 변수가 렌더링에 영향을 끼친다면 (Ex: 실시간으로 증가되는 숫자 변경이라던지) 속성으로 관리

```javascript
import React from 'react';

export class Stopwatch extends React.Component {
    tickRef;


    // 함수가 아닌 속성 임
    tick = () => {
        // isRunning이 true이면 timer를 1씩 증가
        if (this.state.isRunning) {
            this.setState(prevState => ({
                timer: prevState.timer + 1
            }))
        }
    }

    handleClick = () => {
        this.setState(prevState => ({
            isRunning: !prevState.isRunning
        }))
    }

    constructor(props) {
        super(props);
        this.state = {
            timer: 0,
            isRunning: false
        }
    }

    render() {
        return (
          <div className="stopwatch">
              <h2>Stopwatch</h2>
              <span className="stopwatch-time">{this.state.timer}</span>
              <button onClick={this.handleClick}>{this.state.isRunning ? 'Stop' : 'Start'}</button>
              <button>Reset</button>
          </div>
        );
    }

    // DOM이 렌더링 된 직후에 호출되는 라이프 사이클
    // 3rd 라이브러리 로딩, 네트워크 호출
    componentDidMount() {
        this.tickRef = setInterval(this.tick, 1000);
    }

    // DOM이 파괴되기 직전에 호출되는 라이프 사이클
    // 리소스 해제 등등
    componentWillUnmount() {
        clearInterval(this.tickRef);
    }
}

```


## 자바스크립트는 싱글스레드 이다
```javascript
// 코드를 모두 실행하고 마지막에 예약된 큐를 확인해서 실행한다.
// 따라서 실행결과는 항상 동일하다.
// *실행 순서는 항상 A > C > B 동일!!
console.log("A");
//예약
setTimeout(function () { // 해당 조건을 큐에 담아 놓는다.
console.log("B");
}, 0);
console.log("C");

// 만일 루프가돌면?
while(true) {} // 해당 조건문이 실행되면 B는 찍히지 않는다. A > C 만 찍힘

/*
Java 같은 경우는 멀티쓰레드이기 때문에 A찍히고 CPU에 따라 B, C 둘중에 랜덤으로 찍힌다.
*/
```

## ES6 Spread 연산자

새로운 바구니에 담아준다라고 이해하면 쉬움! [(참고)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_operator)
```javascript
const a1 = { x: 1, y: 2 };
const a2 = { ...a1, z: 3 };
console.log(a2); // { x: 1, y: 2, z: 3 }
```
## componentDidMount()
DOM이 렌더링 된 직후에 호출되는 라이플사이클로 3rd 라이브러리 로딩 및 네트워크 호출(API통신)

## componentWillUnmount()
DOM이 파괴되기 직전에 호출되는 라이프사이클로 리소스 해제 등에 사용

## Conditional Rendering
- 조건 및 상황에 따라 원하는 Component를 자바스크립의 조건문과 연산자를 통해 렌더링하는 방법 [(참고)](https://ko.reactjs.org/docs/conditional-rendering.html)

## shallow copy 와 deep copy
객체복사는 크게 얕은 복사(shallow copy)와 깊은 복사(deep copy)로 나뉜다.

복사하려는 값이 string 일때와 object 일때는 다르다는 것이다.

<b>string</b>
```javascript
let str1 = "하이요";
let str2 = str1;
str2 = "잘가요";
console.log(str1); // 하이요
console.log(str2); // 잘가요
```


<b>object</b>

object 일 경우 메모리 주소값만 참조하기 때문에 같이 변경 됨
```javascript
let book = {title: "인사이드 자바스크립트", price: 18000};
let copyBook = book; //
copyBook.price = 20000;
console.log(book);
console.log(copyBook);
```

그렇다면 object 일 경우 어떻게 접근해야 할까?

### 얕은 복사(shallow copy)
```javascript
// @shallow copy

// @deep copy ( es6 spread 연산자를 사용하여 deep copy 하기 )
let book = {title: "인사이드 자바스크립트", price: 18000};
let copyBook = {...book, ...{a:1}, ...{title: 'temp'}};
copyBook.price = 20000;

console.log(book);
// 결과 : {title: "인사이드 자바스크립트", price: 18000};

console.log(copyBook);
// 결과 :  {a: 1, price: 20000, title: "temp"};
```


### 깊은 복사(deep copy)
value 안에 또 다른 object가 있을 경우 조작될 수 있기 때문에 deep copy를 이용해 준다.

순수 자바스크립트로는 불가능하고 써드파티 라이브러리를 이용해서 사용해야 한다.


## Redux
이전 상태와 액션을 받아서 다음 상태를 반환한다. (이전상태를 변경하는 것이 아닌 새로운 상태를 반환하는 것)

기존 상태를 복사 후 액션에 따라 변화를 준 다음에 반환 (복사기 라고 생각해라)


### Install
react 안에 포함되있지 않기 때문에 별도로 설치해야 한다
```javascript
npm install --save redux react-redux
```

### 폴더구조
개발자가마다 스타일이 있지만 여기서는 강사님의 스타일로 구성 했다
- components
- redux
- redux
    - reducers

### react-redux
- view layer binding 도구
- react 컴포넌트에서 리덕스를 사용할떄 복잡한 작업을 react-redux가 다 해준다.

사용방법
- Provider : 컴포넌트에서 리덕스를 사용하도록 서비스를 제공 ( 하나의 컴포넌트 임 )
- connect 함수
    - option을 인수로 받고, 전달받은 option을 사용해서 컴포넌트를 리덕스로 연결하는 "또다른 함수로 반환"
    - 옵션을 전달하지 않았다면 this.props.store로 접근가능

### strore
- 어플리케이션의 현재상태를 지니고 있음
- 리덕스를 사용하는 어플리케이션은 단 하나의 스토어가 있어야 한다.
- store를 만드려면 redux에서 createStore를 불러온 후 reducers 를 인수로 전달하여 해당 함수를 실행하면 된다.

store가 하는 일
- dispatch(action) : action을 리듀서로 보낸다.
    - 리듀서 함수에 state(현재 자신의 상태)와 action(전달받은 액션)을 보낸다.
- getState() : 현재 상태를 반환하는 함수
- subscribe(listener) : 상태가 바뀔때마다 실행 될 callback 함수

### mapStateToProps
react-redux가 제공하는 Helper메소드

```javascript
// store의 state를 props에 매핑 시키겠다.
// 이름이 mapStateToProps 로 거의 고정
// # store에 있는 state.products, state.users 와 연결(subscribe)된다.
// products는 xxx, yyy 아무거나 써도 상관없지만 state.products는 reducers/index.js 의 key값과 같아야 함

const mapStateToProps = (state) => ({ // store로부터 props를 받겠다. (부모에서 받는겐 아니라)
    products: state.products,
    users: state.users
})


// *HoC(Higher-Order Components) : connect( ... ) 에 (App)을 넣었다
// ㄴ> https://reactjs.org/docs/higher-order-components.html
export default connect(mapStateToProps, {updateUser: updateUser})(App);
```
