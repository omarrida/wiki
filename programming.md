# Programming

## Getters

### Information Hiding

Treat objects with lots of getters as a smell. Instead, try to hide information within the class, and expose other affordances. For example:

```php
// Before
if ($order->getStatus() === 'shipped') {...}

// After
if ($order->shipped()) {...}
```

### Avoid Direct Property Access

Try not to directly access properties. Attributes change a lot, and you'll need to update usages across your project and test cases every time the properties change.

```php
// Bad
$user->phone_number;

// Better
$user->getPhoneNumber();
```
