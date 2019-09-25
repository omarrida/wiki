# Laravel

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
*Credit to Moe for teaching me this.*

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
*https://freek.dev/1210-a-better-way-to-register-routes-in-laravel*

To allow easy click-throughs within IDEs, avoid using strings to define controller actions. Instead, use the tuple syntax.

```php
// Bad
Route::get('/posts', 'GetPostsController@index');

// Good
Route::get('/posts', [GetPostsController::class, 'index']);
```

## Parallel Testing
https://medium.com/realblocks-blog/parallel-testing-for-laravel-how-we-cut-test-suite-execution-time-to-1-3-in-one-day-544e458f48ad

TL;DR use the [Paratest](https://github.com/paratestphp/paratest) library for massive performance boosts to your test suite.
