---
title:  Response Macro
permalink: response-macros.html
tags: [laravel]

date: 2019-03-7 00:00:00
keywords: laravel
summary: 사용자의 응답정보(response)를 개발자의 스타일에 맞춰 변형시키는 방법으로 앱 개발자들이 API룰 호출하게 되면 그 결과를 응답정보로 보내어 상황에 맞게 결과값을 파싱할 수 있도록 도와주는 코드 입니다.
box_number: 1
folder: meteopark_programming_laravel
sidebar: category_sidebar
---
## ServiceProvider 생성 

<pre><code>php artisan make:provider ResponseMacroServiceProvider</code></pre>
 
```php

<?php

namespace App\Providers;
use App\Exceptions\MeteoException;
use Illuminate\Support\Facades\Response;
use Illuminate\Support\ServiceProvider;

class ResponseMacrosServiceProvider extends ServiceProvider
{
    /**
     * Register services.
     *
     * @return void
     */


    public function register()
    {
        //
    }

    /**
     * Bootstrap services.
     *
     * @return void
     */
    public function boot()
    {

        // 실패일때
        Response::macro('error', function(MeteoException $e)
        {

            $request = app(\Illuminate\Http\Request::class);

            $response = [

                'stat' => 0,
                'error' => [

                    'code' => $e->getCode(),
                    'message' => $e->getMessage(),
                ],
            ];

            if(!empty($request->get('callback'))){

                return Response()->json($response)->setCallback( $request->get('callback') );

            }else{

                return Response()->json($response);
            }
        });

        // 성공일때
        Response::macro('success', function ($result = null) {

            $response = [
                'stat' => 0,
                'response' => new \stdClass(),
            ];

            if(!empty($result)) $response['response'] = $result;

            return Response()->json($response);

        });
    }



}
```
`macro()` 함수의 첫 번째 인수 `success` 는 호출할 함수명 입니다. 두 번째 인수 `$result` 는 Android/iOS에 전달할 값으로 `$response['response']` 에 merge 시켜 보낼 것 입니다.
만약 전달할 값이 없다면 `$response['response']`에는 `\stdClass()` 를 기본으로 보내 줄 것입니다.

`$response['stat']` 의 값은 0(성공)과 1(실패) 두 가지로 분류했습니다. 

- `stat : 1` 인 경우 생성한 [`Custom Exception`](https://github.com/meteopark/laravel-core/blob/master/guide/response-custom-exception.md) 을 통해 예외 처리를 해 줄 것입니다.
 

## ServiceProvider 등록 


`app.php` 에 생성한 `ResponseMacrosServiceProvider` 를 등록 합니다.
```
'providers' => [

        /*
         * Laravel Framework Service Providers...
         */
        Illuminate\Auth\AuthServiceProvider::class,
        Illuminate\Broadcasting\BroadcastServiceProvider::class,
        Illuminate\Bus\BusServiceProvider::class,
        Illuminate\Cache\CacheServiceProvider::class,
        Illuminate\Foundation\Providers\ConsoleSupportServiceProvider::class,
        Illuminate\Cookie\CookieServiceProvider::class,
        Illuminate\Database\DatabaseServiceProvider::class,
        Illuminate\Encryption\EncryptionServiceProvider::class,
        Illuminate\Filesystem\FilesystemServiceProvider::class,
        Illuminate\Foundation\Providers\FoundationServiceProvider::class,
        Illuminate\Hashing\HashServiceProvider::class,
        Illuminate\Mail\MailServiceProvider::class,
        Illuminate\Notifications\NotificationServiceProvider::class,
        Illuminate\Pagination\PaginationServiceProvider::class,
        Illuminate\Pipeline\PipelineServiceProvider::class,
        Illuminate\Queue\QueueServiceProvider::class,
        Illuminate\Redis\RedisServiceProvider::class,
        Illuminate\Auth\Passwords\PasswordResetServiceProvider::class,
        Illuminate\Session\SessionServiceProvider::class,
        Illuminate\Translation\TranslationServiceProvider::class,
        Illuminate\Validation\ValidationServiceProvider::class,
        Illuminate\View\ViewServiceProvider::class,

        /*
         * Package Service Providers...
         */

        /*
         * Application Service Providers...
         */
        App\Providers\AppServiceProvider::class,
        App\Providers\AuthServiceProvider::class,
        // App\Providers\BroadcastServiceProvider::class,
        App\Providers\EventServiceProvider::class,
        App\Providers\RouteServiceProvider::class,
        App\Providers\ResponseMacrosServiceProvider::class, // <<<<<< 여기요!!

    ],
```
`web.php` 에 코드를 넣고 실행해 봅니다.
```
Route::get('/', function () {
    
    // return \Response::success($데이터); // 데이터를 전달하고 싶을 시 Array 형태로 넣어준다. 
    return \Response::success();
});

```

결과화면 
```
{ // $result 가 빈 값일 때
    stat: 0,
    response: { },
}
```
or
```
{ // $result 에 데이터가 있을 때
    stat: 0,
    response: {
        name: 'meteopark',
        age : 25,
        image : {
            path: 'http://rzip84.blog.me/~~~.png',
            width: 1024,
            height: 500
        }
         
    },
}
```


## 마치며
`Array` 결과값을 `return` 시 어차피 `json` 으로 갈 것인데 왜 굳이 `Response()->json($데이터)` 로 처리했을까? 

`stdClass` 경우 응답정보를 받는 곳에서 `content-type` 을 `application/json` 으로 인식 하는 것이 아닌 `text/html` 로 인식하게 된다.  

- - -
- [목록으로 돌아가기](https://github.com/meteopark/laravel-core)
- [사용자 정의 예외처리 만들기 - Custom Exception](https://github.com/meteopark/laravel-core/blob/master/guide/response-custom-exception.md)

{% include links.html %}
