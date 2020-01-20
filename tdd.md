# TDD

## Custom WithFactories Trait

When you call factories directly in your tests like `factory(User::class)->create()`, you end up violating DRY because you'll need an authed user for many of your tests. The cost of violating DRY here is that if your definition of an authed user changes, you'll need to update all the factory calls.

For example, let's say the expectation of an authed user on your platform changes to include a related `Address` and `Phone`. We would now have to add two more factory calls everywhere an authed user is needed.

```php
// Before, not bad
$user = factory(User::class)->create();

$this->actingAs($user)->post('/login')->assertOk();

// Now, with the added requirements. You need to make this change in all your tests. Worse, what happens if the Address or Phone no longer has a `user_id` foreign?
$user = factory(User::class)->create();
$address = factory(Address::class)->create(['user_id' => $user->id]);
$phone = factory(Phone::class)->create(['user_id' => $user->id]);

$this->actingAs($user)->post('/login')->assertOk();
```

The solution to this problem: Create a trait called `WithFactories` that you can import in your test cases. Instead of calling factories directly, create methods on this trait that express high-level intent, instead of factories and specific model names.

```php
$user = $this->createUser();

$this->actingAs($user)->post('/login')->assertOk();
```

This way, if your requirements for a user change, you only need to update the `createUser()` method in your `WithFactories` trait. Your intent of creating a user that is able to access your platform is still valid, so your test cases don't need to change.

## Custom ApiHelpers Trait

Your API routes will change over time, and you need to update those references in your test cases as well. Same as the `WithFactories` trait, try to reference API calls by method name and stay DRY. You can just copy the ApiHelpers trait and write nice public method names for yourself like `login()` or `$this->actingAs($user)->updateProfilePicture()`.

```php
/**
 * Trait ApiHelpers
 * @package Tests\Utilities
 */
trait ApiHelpers
{
    /**
     * @param string $route
     * @param array $headers
     * @return TestResponse
     */
    protected function getApi(string $route, array $headers = []): TestResponse
    {
        $headers = array_merge($headers, $this->defaultHeaders);

        return $this->get("/api$route", $headers);
    }

    /**
     * @param string $route
     * @param array $body
     * @param array $headers
     * @return TestResponse
     */
    protected function postApi(string $route, array $body = [], array $headers = []): TestResponse
    {
        $headers = array_merge($headers, [
            'Content-Type' => 'application/x-www-form-urlencoded'
        ]);

        return $this->post("/api$route", $body, $headers);
    }

    /**
     * @param string $route
     * @param array $body
     * @param array $headers
     * @return TestResponse
     */
    protected function putApi(string $route, array $body = [], array $headers = []): TestResponse
    {
        $headers = array_merge($headers, [
            'Content-Type' => 'application/x-www-form-urlencoded'
        ]);

        return $this->put("/api$route", $body, $headers);
    }
    ```
