# Laravel

## Deployments

[Forge](https://forge.laravel.com) is the best way to deploy Laravel apps.

[Vapor](https://vapor.laravel.com) is a new way to deploy the app as managed Lamda functions. Serverless PHP is pretty awesome and very easy to scale. Fantastic choice for startups that want to push off the worries of scale and focus on building a great product experience.

> I always forget to add the dynamodb cache driver when I upgrade from 5.8 to 6 and want to use vapor. Here's the link so I don't have to dig through my history:

https://codinglabs.com.au/blog/2019/8/under-the-hood-with-laravel-vapor

TL;DR is add this to the `cache.php` file:

```
'dynamodb' => [
  'driver' => 'dynamodb',
  'key' => env('AWS_ACCESS_KEY_ID'),
  'secret' => env('AWS_SECRET_ACCESS_KEY'),
  'region' => env('AWS_REGION', 'us-east-1'),
  'table' => env('DYNAMODB_CACHE_TABLE', 'cache'),
],
```

## Custom Aliases

```bash
alias art='php artisan'
alias lmm='php artisan make:model --migration'
alias ide='php artisan ide-helper:generate && php artisan ide-helper:models && php artisan ide-helper:meta'
alias db='php artisan migrate:fresh --seed'
alias test='vendor/bin/phpunit'
alias ptest='vendor/bin/paratest -p16 --runner=WrapperRunner'
```

## Invokable Controllers

For extra simple controllers, you can define an invokable controller action in the routes file:

```php
Route::get('/posts', GetPostsController::class);
```

Then in your controller class:

```php
class GetPostsController {
  public function __invoke() {
    // Do something
  }
}
```

## Single-Line Controllers

> Note: This method has a tradeoff - if you throw all your controller logic in the request, your controller becomes thin, but you've just delegated model logic to the FormRequest. All model-related logic should be at the model layer, not the request. This helped me for several months, and then I refactored away from it as my domain understanding became more mature.

Extend `FormRequest` for all controller actions to encapsulate authorization and validation. Include methods `handle()` and `response()` which further encapsulate the handling of the request and formatting the response respectively.

```php
class PostController {
  public function index(GetPostsRequest $request)
  {
    return $request->handle()->response();
  }
}
```

## IDE-Friendly Routes

*[Credit to Freek](https://freek.dev/1210-a-better-way-to-register-routes-in-laravel)*

To allow easy click-throughs within IDEs, avoid using strings to define controller actions. Instead, use the tuple syntax.

```php
// Bad
Route::get('/posts', 'GetPostsController@index');

// Good
Route::get('/posts', [GetPostsController::class, 'index']);

// Good
Route::get('/posts', GetPostsController::class);
```

## Parallel Testing
https://medium.com/realblocks-blog/parallel-testing-for-laravel-how-we-cut-test-suite-execution-time-to-1-3-in-one-day-544e458f48ad

TL;DR use the [Paratest](https://github.com/paratestphp/paratest) library for massive performance boosts to your test suite.
