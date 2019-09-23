# Laravel

## Single-Line Controllers

Extend `FormRequest` for all controller actions to encapsulate authorization and validation. Include methods `handle()` and `response()` which further encapsulate the handling of the request and formatting the response respectively.

```
class PostController {
  public function index(GetPostsRequest $request)
  {
    return $request->handle()->response();
  }
}
```
