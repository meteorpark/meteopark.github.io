---
title:  Laravel 5 - Custom Exception
permalink: 2019-03-18-response_custom_exception.html
tags: [laravel]
keywords: laravel, custom exception, exception
date: 2019-03-18 00:00:00
summary: 요청받은 정보에 이슈가 생겼을 시 예외 발생을 Custom 하게 변경시켜주는 코드입니다.
folder: meteopark_programming_laravel
sidebar: category_sidebar
search: exclude
---

## Exception 생성 
<pre><code>php artisan make:exception MeteoException</code></pre>
 
생성된 클래스에 `customExceptionCode()` 를 추가하여 예외 코드들을 넣어 놓습니다.

```php
<?php

namespace App\Exceptions;

use Exception;

class MeteoException extends Exception
{

    private $exceptionInfo = [];


    /**
     * MeteoException constructor.
     * @param Int $code
     * @param String $message
     * @param Exception|null $previous
     */
    public function __construct(Int $code = 999, String $message = "", Exception $previous = null){

        $this->customExceptionCode($code, $message);

        parent::__construct( $this->exceptionInfo['message'], $this->exceptionInfo['code'], $previous );

    }


    /**
     * @param Int $code         들어오는 코드와 부합하지 않을 시 999 호출
     * @param String $message
     */
    private function customExceptionCode(Int $code, String $message){

        switch($code) {

            case 800 :

                $this->exceptionInfo['code'] = $code;
                $this->exceptionInfo['message'] = '알 수 없는 회원 입니다.';
                break;

            case 801 :
                $this->exceptionInfo['code'] = $code;
                $this->exceptionInfo['message'] = '탈퇴한 회원 입니다.';
                break;

            default :

                $this->exceptionInfo['code'] = 999;
                $this->exceptionInfo['message'] = '알 수 없는 에러입니다.';
                break;
        }

        if(!empty($message)) $this->exceptionInfo['message'] = $message;
    }
}



```





## Handler 에  등록 
`app/Exceptions/Handler.php` 의 `render()` 를 수정 합니다.

```php
<?php

namespace App\Exceptions;

use Exception;
use Illuminate\Foundation\Exceptions\Handler as ExceptionHandler;
use Illuminate\Support\Facades\Response;
use App\Exceptions\MeteoException;

class Handler extends ExceptionHandler
{
    /**
     * A list of the exception types that are not reported.
     *
     * @var array
     */
    protected $dontReport = [
        //
    ];

    /**
     * A list of the inputs that are never flashed for validation exceptions.
     *
     * @var array
     */
    protected $dontFlash = [
        'password',
        'password_confirmation',
    ];

    /**
     * Report or log an exception.
     *
     * @param  \Exception  $exception
     * @return void
     */
    public function report(Exception $exception)
    {
        parent::report($exception);
    }

    /**
     * Render an exception into an HTTP response.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Exception  $exception
     * @return \Illuminate\Http\Response
     */
    public function render($request, Exception $exception)
    {
        if($exception instanceof MeteoException)
        {

            return Response::error($exception);

        }
        return parent::render($request, $exception);
    }
}


```

## Custom Exception 사용하기 
```php

<?php


Route::get('/', function () {

    throw new \App\Exceptions\MeteoException(100);

});


```
요청한 코드 값이 등록되어 있지 않다면, 미리 생성한 `999`  에러를 호출하게 됩니다.

- - -
- [목록으로 돌아가기](https://github.com/meteopark/laravel-core)
- [사용자 정의 응답정보 만들기 - Response Macro](https://github.com/meteopark/laravel-core/blob/master/guide/response-macros.md)

{% include links_detail.html %}