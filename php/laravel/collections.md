# PHP / Laravel / Collections

- [Collections - Laravel Documentation](http://laravel.com/docs/5.1/collections)

## transform()

- iterates over a collection calling the given callback on each item
- replaces the item value with the returned callback value
- modifies the collection itself

```php
$collection = collect([1, 2, 3, 4, 5]);

$collection->transform(function ($item, $key) {
    return $item * 2;
});

$collection->all();

// [2, 4, 6, 8, 10]
```
