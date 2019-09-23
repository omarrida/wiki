# Laravel

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
