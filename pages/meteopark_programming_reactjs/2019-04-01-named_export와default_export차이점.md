---
title: ReactJS - named export 와 default export 차이점
permalink: 2019-04-01-named_export와default_export.html
tags: [reactjs, es6]
keywords: reactjs, es6
date: 2019-03-27 00:00:00
summary: export에는 default export 및 named export 의 차이점에 대해 알아본다.
folder: meteopark_programming_reactjs
sidebar: category_sidebar
search: exclude
---
export에는 default export 및 named export 2가지 방법이 있다.

## named export
정의된(const) 이름으로 사용해야 하며, 하나의 js파일에서 여러 개의 값을 export 시킬 수 있다.

```javascript
// action.js
export const UPDATE_TITLE = 'player/UPDATE_TITLE';
export const ADD_PLAYER = 'player/ADD_PLAYER';

// Header.js
import {updateTitle, ADD_PLAYER} from "./action";
```

import 시 { 중괄호 } 를 사용해야 한다.

```javascript
import { A } from './A'

export const A = 42
// or
export class A = 100
```

## default export
js파일 내에 하나만 생성해야 하며, import시 아무 이름으로 가져와도 된다.
```javascript
// A.js
export default 42

// CASE1 B.js
import ABCDEFG from './A'

// CASE2 B.js
import A from './A'
import MyA from './A'
import Something from './A'
```

출처 : https://stackoverflow.com/questions/36795819/when-should-i-use-curly-braces-for-es6-import/36796281#36796281

{% include links_detail.html %}