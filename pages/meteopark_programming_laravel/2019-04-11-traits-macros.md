---
title:  PHP - 트레이트 (Trait)
permalink: 2019-04-11-traits-macros.html
tags: [laravel]
keywords: laravel, php, trait
date: 2019-04-11 00:00:00
summary: PHP5.4부터 추가된 Traits 라는 기능에 대해 알아보자.
folder: meteopark_programming_laravel
sidebar: category_sidebar
search: exclude
---
## Trait 란?
PHP5.4부터

정광섭님이 작성하신 Trait 내용 중 인상깊은 문구가 있다. "인터페이스라는 어려운 용어보다는 계약이 더 명확한 의미를 전달한다고 생각합니다." 이제 주변에서 인터페이스를 묻는다면 이처럼 말해 줄 것이다.

그럼 언제 Trait 를 사용해야 할까?







## 마치며
`Array` 결과값을 `return` 시 어차피 `json` 으로 갈 것인데 왜 굳이 `Response()->json($데이터)` 로 처리했을까? 

`stdClass` 경우 응답정보를 받는 곳에서 `content-type` 을 `application/json` 으로 인식 하는 것이 아닌 `text/html` 로 인식하게 된다.  

- - -
- [사용자 정의 예외처리 만들기 - Custom Exception](https://github.com/meteopark/laravel-core/blob/master/guide/response-custom-exception.md)

{% include links_detail.html %}

