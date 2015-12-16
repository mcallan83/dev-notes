# PHP / Laravel / Eloquent

## One To One Relationship

- a user that has one phone

```php
class User extends Model
{
    public function phone()
    {
        return $this->hasOne('App\Phone');
    }
}
```

```php
class Phone extends Model
{
    public function user()
    {
        return $this->belongsTo('App\User');
    }
}
```


## Querying Relationships -  Multiple Levels

- [Laravel – querying any level far relations with simple trick](http://softonsofa.com/laravel-querying-any-level-far-relations-with-simple-trick/)
- queries any level of nesting and any type of relation

```php
$user->load(['organizations.properties' => function ($q) use ( &$properties ) {
   $properties = $q->get()->unique();
}]);

$properties; // the collection we needed
```
