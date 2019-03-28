---
title: Laravel 5 - JWT 1.0.* 사용하기
permalink: jwt.html
tags: [laravel, jwt]
keywords: laravel, jwt, jwt1.0.*
date: 2019-03-20 00:00:00
summary: 해당 가이드는 Laravel 5.8 및 JWT 1.0.* 기준으로 작성되었습니다.
box_number: 1
folder: meteopark_programming_laravel
sidebar: category_sidebar
---
### 1. JWT 설정
#### 1-1. 설치
JWT는 해당 [패키지](https://github.com/tymondesigns/jwt-auth/)를 통해 설치합니다.
 
`composer require "tymon/jwt-auth:1.0.*"`

#### 1-2. ServiceProvider 와 Facade 를 추가
```php
// config/app.php

'providers' => [
    ...
    Tymon\JWTAuth\Providers\LaravelServiceProvider::class,
],

'aliases' => [
    ...
    'JWTAuth' => Tymon\JWTAuth\Facades\JWTAuth::class,
    'JWTFactory' => Tymon\JWTAuth\Facades\JWTFactory::class,
],
```
#### 1-3. config file 배포 및 JWT 키 변경
```
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
php artisan jwt:generate
```



### 2. Middleware
#### 2-1. 사용자 정의 Middleware
사용자 기반의  응답 정보로 변경하기 위해 새로운 Middleware를 생성한다.

`php artisan make:middleware CustomGetUserToken`<br>
```php
// app/Http/Middleware/CustomGetUserToken.php

namespace App\Http\Middleware;

use Closure;
use Tymon\JWTAuth\Exceptions\JWTException;
use Tymon\JWTAuth\Http\Middleware\BaseMiddleware;

class CustomGetUserToken extends BaseMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request $request
     * @param  \Closure $next
     * @return mixed
     * @throws JWTException
     */
    public function handle($request, Closure $next)
    {

        if (! $this->auth->setRequest($request)->getToken()) {

            throw new JWTException('Token not provided', 400);
        }

        if (! $this->auth->authenticate($request)) {

            throw new JWTException('User not found', 404);
        }

        return $next($request);
    }
```
#### 2-2. Kernel 에 등록
```php
<?php 
// app/Http/Kernel.php

namespace App\Http;

use Illuminate\Foundation\Http\Kernel as HttpKernel;

class Kernel extends HttpKernel
{
    ... 
    
    protected $routeMiddleware = [
        ...
        'jwt.auth'    => \App\Http\Middleware\CustomGetUserToken::class,
        'jwt.refresh' => \Tymon\JWTAuth\Http\Middleware\Authenticate::class,
    ];
```
### 3. Controller 생성
해당 예제에서는 JWT 에서 기본적으로 제공해주는 id, eamil의 인증 말고 사용자 기반의 토큰을 만들어 줄 것이다.

```
php artisan make:controller Test/JwtController
php artisan make:model /Models/User
```
토큰을 얻어오기 위한 함수 `join()` 과 `login()` 을 생성해 준다.

```php
// app/Http/Test/JwtController.php

namespace App\Http\Controllers\Test;

use Illuminate\Http\Request;
use App\Http\Controllers\Controller;
use Illuminate\Support\Facades\Response;
use App\Models\User;
use JWTAuth;
use JWTFactory;

class JwtController extends Controller
{

    protected function join(Request $request)
    {

        $user = User::where('name', $request->get('name'))->first();

        $token = JWTAuth::fromUser($user);

        return Response::success(['token' => $token]);
    }

    protected function login(){

        $user = JWTAuth::parseToken()->authenticate();

        return Response::success($user);
    }
}
```
### 4. 인증 테이블 변경
default 테이블이 아닌, 앞서 만든 User 테이블에서 인증하도록 할 것 이다.
   
```php
// config/auth.php

    return[ 
        ...
        'providers' => [
            'users' => [
                'driver' => 'eloquent',
                'model' => App\Models\User::class,
            ],
        ],
    ]
```


### 5. Exception 처리
JWT 에서 제공하는 Exception 을 사용자 정의에 따라 처리 할 것이다.

Custom Exception을 만드는 방법은 이전에 작성한 문서를 참고한다.

- [사용자 정의 응답정보 만들기 - Response Macro](https://github.com/meteopark/laravel-core/blob/master/guide/response-macros.md) 
- [사용자 정의 예외처리 만들기 - Custom Exception](https://github.com/meteopark/laravel-core/blob/master/guide/response-custom-exception.md)

#### 5-1. Handler 수정
```php
// app/Exceptions/Handler.php

namespace App\Exceptions;

use Exception;
use Illuminate\Foundation\Exceptions\Handler as ExceptionHandler;
use Illuminate\Support\Facades\Response;
use App\Exceptions\MeteoException;
use Tymon\JWTAuth\Exceptions\JWTException;
use Tymon\JWTAuth\Exceptions\TokenExpiredException;
use Tymon\JWTAuth\Exceptions\TokenInvalidException;

class Handler extends ExceptionHandler
{
    ...
    
    public function render($request, Exception $exception)
    {

        if ($exception instanceof TokenExpiredException) {

            return Response::jwt_error($exception->getCode(), $exception->getMessage());

        }else if($exception instanceof TokenInvalidException){

            return Response::jwt_error($exception->getCode(), $exception->getMessage());

        }else if($exception instanceof JWTException){

            return Response::jwt_error($exception->getCode(), $exception->getMessage());

        }

        if($exception instanceof MeteoException)
        {

            return Response::error($exception);

        }
        return parent::render($request, $exception);
    }
}
```
그리고 `ResponseMacrosServiceProvider` 에 `Response::jwt_error()` 함수를 추가 한다.

```php
// app/Providers/ResponseMacrosServiceProvider.php

namespace App\Providers;
use App\Exceptions\MeteoException;
use Illuminate\Support\Facades\Response;
use Illuminate\Support\ServiceProvider;

class ResponseMacrosServiceProvider extends ServiceProvider
  
    public function boot()
    {
        ...

        // 성공일때
        Response::macro('jwt_error', function ($code, $message) {

            $response = [

                'stat' => 0,
                'error' => [

                    'code' => $code,
                    'message' => $message,
                ],
            ];

            return Response()->json($response);

        });
    }
}
```

### 6. 테스트
```php
// routes/web.php

Route::get('v1/jwt/join', array(
    'as' => 'v1.jwt.join',
    'uses' => 'Test\JwtController@join'
));

Route::middleware(['jwt.auth'])->group(function () {

    Route::get('v1/jwt/login', array(
        'as' => 'v1.jwt.login',
        'uses' => 'Test\JwtController@login'
    ));
});

```
가입 했을때

![success =200x100](images/md-images/success.png)


로그인 했을때(사용자 정보 가져 옴)

![login =200x100](images/md-images/login.png )


### 마치며
이전 프로젝트(Laravel 5.2) 에서 사용한 JWT를 Laravel 5.8에서 적용해 보았다.

참고만 하겠다던 김주원님의 [강의내용](https://github.com/appkr/l5essential/blob/master/lessons/46-jwt.md)과 너무 비슷해 보인다.

JWT 관련 한 이론은 위 링크에서 확인하길 바란다.
 

- - -
- [목록으로 돌아가기](https://github.com/meteopark/laravel-core)
