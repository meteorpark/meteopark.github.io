<!-- --- -->
<!-- title:  GraphQL 과 Laravel 연동하기 #1 - 설치 후 데이터 조회하기 -->
<!-- permalink: 2019-07-10-graphql-laravel.html -->
<!-- tags: [laravel, graphql] -->
<!-- keywords: laravel, graphqlmacros -->
<!-- date: 2019-07-10 00:00:00 -->
<!-- summary: Laravel(5.8)과 GraphQL 연동하기 입니다. 이미 라라벨용으로 구현된 소스가 존재하나 설치 시 막혔던 부분에 대해 정리하고자 합니다. -->
<!-- folder: meteopark_programming_laravel -->
<!-- sidebar: category_sidebar -->
<!-- search: exclude -->
<!-- --- -->
<!-- ## 1. Laravel용 GraphQL 적용하기 -->
<!-- 자세한 내용은 [바로가기](https://github.com/rebing/graphql-laravel) 를 통해 확인 하세요. 자세히 설명 되어 있습니다. -->

<!-- ### 1-1 설치하기 -->
<!-- <pre><code>php artisan make:provider ResponseMacroServiceProvider</code></pre> -->

<!-- #### 1-1-1. Laravel 5.5 이전 버전의 경우 `config/app.php` 에 다음 서비스 공급자를 추가합니다. -->
<!-- <pre><code>Rebing\GraphQL\GraphQLServiceProvider::class</code></pre> -->

<!-- #### 1-1-2. publish를 진행하게 되면 `config/graphql.php` 라는 파일이 생성됩니다. -->
<!-- <pre><code>php artisan vendor:publish --provider="Rebing\GraphQL\GraphQLServiceProvider"</code></pre> -->

<!-- ## 2. 설정하기 -->

<!-- ### `config/graphql.php` 의 내용은 아래와 같다. -->



<!-- - `stat : 1` 인 경우 생성한 [`Custom Exception`](https://github.com/meteopark/laravel-core/blob/master/guide/response-custom-exception.md) 을 통해 예외 처리를 해 줄 것입니다. -->
































<!-- ## ServiceProvider 등록  -->


<!-- `app.php` 에 생성한 `ResponseMacrosServiceProvider` 를 등록 합니다. -->
<!-- ``` -->
<!-- 'providers' => [ -->

<!--         /* -->
<!--          * Laravel Framework Service Providers... -->
<!--          */ -->
<!--         Illuminate\Auth\AuthServiceProvider::class, -->
<!--         Illuminate\Broadcasting\BroadcastServiceProvider::class, -->
<!--         Illuminate\Bus\BusServiceProvider::class, -->
<!--         Illuminate\Cache\CacheServiceProvider::class, -->
<!--         Illuminate\Foundation\Providers\ConsoleSupportServiceProvider::class, -->
<!--         Illuminate\Cookie\CookieServiceProvider::class, -->
<!--         Illuminate\Database\DatabaseServiceProvider::class, -->
<!--         Illuminate\Encryption\EncryptionServiceProvider::class, -->
<!--         Illuminate\Filesystem\FilesystemServiceProvider::class, -->
<!--         Illuminate\Foundation\Providers\FoundationServiceProvider::class, -->
<!--         Illuminate\Hashing\HashServiceProvider::class, -->
<!--         Illuminate\Mail\MailServiceProvider::class, -->
<!--         Illuminate\Notifications\NotificationServiceProvider::class, -->
<!--         Illuminate\Pagination\PaginationServiceProvider::class, -->
<!--         Illuminate\Pipeline\PipelineServiceProvider::class, -->
<!--         Illuminate\Queue\QueueServiceProvider::class, -->
<!--         Illuminate\Redis\RedisServiceProvider::class, -->
<!--         Illuminate\Auth\Passwords\PasswordResetServiceProvider::class, -->
<!--         Illuminate\Session\SessionServiceProvider::class, -->
<!--         Illuminate\Translation\TranslationServiceProvider::class, -->
<!--         Illuminate\Validation\ValidationServiceProvider::class, -->
<!--         Illuminate\View\ViewServiceProvider::class, -->

<!--         /* -->
<!--          * Package Service Providers... -->
<!--          */ -->

<!--         /* -->
<!--          * Application Service Providers... -->
<!--          */ -->
<!--         App\Providers\AppServiceProvider::class, -->
<!--         App\Providers\AuthServiceProvider::class, -->
<!--         // App\Providers\BroadcastServiceProvider::class, -->
<!--         App\Providers\EventServiceProvider::class, -->
<!--         App\Providers\RouteServiceProvider::class, -->
<!--         App\Providers\ResponseMacrosServiceProvider::class, // <<<<<< 여기요!! -->

<!--     ], -->
<!-- ``` -->
<!-- `web.php` 에 코드를 넣고 실행해 봅니다. -->
<!-- ``` -->
<!-- Route::get('/', function () { -->
<!--      -->
<!--     // return \Response::success($데이터); // 데이터를 전달하고 싶을 시 Array 형태로 넣어준다.  -->
<!--     return \Response::success(); -->
<!-- }); -->

<!-- ``` -->

<!-- 결과화면  -->
<!-- ``` -->
<!-- { // $result 가 빈 값일 때 -->
<!--     stat: 0, -->
<!--     response: { }, -->
<!-- } -->
<!-- ``` -->
<!-- or -->
<!-- ``` -->
<!-- { // $result 에 데이터가 있을 때 -->
<!--     stat: 0, -->
<!--     response: { -->
<!--         name: 'meteopark', -->
<!--         age : 25, -->
<!--         image : { -->
<!--             path: 'http://rzip84.blog.me/~~~.png', -->
<!--             width: 1024, -->
<!--             height: 500 -->
<!--         } -->
<!--           -->
<!--     }, -->
<!-- } -->
<!-- ``` -->


<!-- ## 마치며 -->
<!-- `Array` 결과값을 `return` 시 어차피 `json` 으로 갈 것인데 왜 굳이 `Response()->json($데이터)` 로 처리했을까?  -->

<!-- `stdClass` 경우 응답정보를 받는 곳에서 `content-type` 을 `application/json` 으로 인식 하는 것이 아닌 `text/html` 로 인식하게 된다.   -->

<!-- - - - -->
<!-- - [목록으로 돌아가기](https://github.com/meteopark/laravel-core) -->
<!-- - [사용자 정의 예외처리 만들기 - Custom Exception](https://github.com/meteopark/laravel-core/blob/master/guide/response-custom-exception.md) -->

<!-- {% include links_detail.html %} -->

