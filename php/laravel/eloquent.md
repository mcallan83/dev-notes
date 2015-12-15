# PHP / Laravel / Eloquent

# Querying Relationships -  Multiple Levels

- [Laravel – querying any level far relations with simple trick](http://softonsofa.com/laravel-querying-any-level-far-relations-with-simple-trick/)
- queries any level of nesting and any type of relation

```php
$user->load(['users.organizations.properties' => function ($q) use ( &$properties ) {
   $properties = $q->get()->unique();
}]);

$properties; // the collection we needed
```
