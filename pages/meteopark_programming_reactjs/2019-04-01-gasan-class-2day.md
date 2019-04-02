---
title: ReactJS - 교육 2일차 - 모듈 방식 개발
permalink: 2019-04-01-gasan-class-2day.html
tags: [reactjs]
keywords: reactjs
date: 2019-04-01 00:00:00
summary: React.js 2일차에 배운 내용을 정리한다.
folder: meteopark_programming_reactjs
sidebar: category_sidebar
search: exclude
---

## state를 초기화 하는 방법
* 생성자일때는 this.state
* 생성자가 아닐때는 state


## NPM과 WEBPACK
* npm : node package manager 로 모듈 관리자이다.
* webpack : 여러가지 모듈을 모아서 하나로 만들어주는 모듈 번들러이다.


## 프로젝트 생성
```javascript
// 버전관리 없이 원격으로 가져올 수 있어서 npx로 설치하는게 요즘 추세임
npx create-react-app scoreboard
```

## node_modules
해당 모듈은 형상관리(ex : git)하는것이 아닌 package.json에만 명시해 둔다.

다른곳에서 소스를 받은 뒤 npm install로 설치한다.

## state 접근방법
- 생성자일때는 this.state
- 생성자가 아닐때는 state
