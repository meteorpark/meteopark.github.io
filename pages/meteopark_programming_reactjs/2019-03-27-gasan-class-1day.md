---
title: ReactJS - 교육 1일차 - CDN 페이징 방식 개발
permalink: 2019-03-27-gasan-class-1day.html
tags: [reactjs]
keywords: reactjs
date: 2019-03-27 00:00:00
summary: React.js 1일차로 배운 내용을 정리한다.
folder: meteopark_programming_reactjs
sidebar: category_sidebar
search: exclude
---
## React 소개
React는 UI 컴포넌트 라이브러리이다.<br>
단방향 바인딩이다.( 위에서 아래로 )

## 설치

VsCode

[크롬 확장팩인 React 개발도구](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)

[크롬 확장팩인 Redux 개발도구](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en)

## ES6 문법

드디어 `javascript` 도 `class` 라는 개념이 생겼습니다.<br>
이 중 수업시간에 다룬 내용 중 중요한 부분을 정리했다.

## Arrow function
```javascript
let foo = () => { ... }

let foo = e => { ... } // 괄호 생략가능

let foo = (e, x) => { ... } // 매개변수가 2개 이상인 경우 
``` 

## Array Method map
[`map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 은 MDN에 있는 내용으로 살펴보면 <br>
`The map() method creates a new array with the results of calling a provided function on every element in the calling array.`<br>
영문 그대로 "기존 배열을 `새로운 배열로 반환` 시킨다"라는 것이 가장 중요하다.

```javascript
var array1 = [1, 4, 9, 16];

// pass a function to map
const map1 = array1.map(x => x * 2);

console.log(map1);
// expected output: Array [2, 8, 18, 32]

Result : > Array [2, 8, 18, 32]
```
## props
컴포넌트와 컴포넌트 사이에 data를 전달할 때 사용된다.
```js
const App = () => {
  return (
    <div className="scoreboard">
      <Header title="My scoreboard" totalPlayers={1 + 10} />
      <Player />
    </div>
  );
}
```

```js
const Header = (props) => {
  return (
    <header>
      <h1>{ props.title }</h1>
      <span className="stats">Players: { props.totalPlayers }</span>
    </header>
  )
}
// 실행형 함수 Header 의 Players 에는 11 이라는 값이 들어 온다. 
```

## state
`state` 의 가장 중요한 점은 `read only` 라는 것 이다. <br>
위에서 언급한 대로 React는 `단방향 바인드` 이기 때문에 `자식의 자식의 ... 자식` 등으로 넘긴 데이터에 대해서는 임의적으로 변경 시키면 안 된다.
```js
class Counter extends React.Component {
  
  constructor() { // 생성자 함수 포함 시 

    super();

    this.state = {

      score: 0
    }
  }

//   state = { // 생성자 함수 미 포함 시 
//     score: 0
//   };
  
  render() {
    return (
      <div className="counter">
        <button className="counter-action decrement"> - </button>
        <span className="counter-score">{this.state.score}</span>
        <button className="counter-action increment" onClick={this.incrementScore}> + </button>
      </div>
    );
  }
}
```
대신 `setState` 를 통해 데이터를 `변경` 시킬 수 있다.<br>
하지만 이 부분도 `자식의 자식의 ... 자식`이 돼버린다면 데이터 변경 시 불편한 사항이 발생된다.<br>
이를 해결하기 위해 다음 강의 때는 `Redux` 라는 개념을 배울 것이다.

## Click Event
+버튼 클릭 시 score를 증가시키는 이벤트 이다. 위에서 작성한 `onClick` 예제를 참고한다. 
```js
  incrementScore = () => {
    this.setState(
      { score: this.state.score - 1 }
    );
  }
```


{% include links_detail.html %}