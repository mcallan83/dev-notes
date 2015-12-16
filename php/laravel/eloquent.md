# PHP / Laravel / Eloquent

- [Defining Relationships](#defining-relationships)
    + [One To One](#one-to-one)
    + [One To Many](#one-to-many)
    + [Many To Many](#many-to-many)

## Defining Relationships

### One To One

- a user that has one phone

```php
class User extends Model
{
    public function phone()
    {
        return $this->hasOne('App\Phone');
    }
}

class Phone extends Model
{
    public function user()
    {
        return $this->belongsTo('App\User');
    }
}
```

### One To Many

- a post that has multiple comments

```php
class Post extends Model
{
    public function comments()
    {
        return $this->hasMany('App\Comment');
    }
}

class Comment extends Model
{
    public function post()
    {
        return $this->belongsTo('App\Post');
    }
}
```

### Many To Many

- multiple users that can have multiple roles
- requires joiner table

```php
class User extends Model
{
    public function roles()
    {
        return $this->belongsToMany('App\Role');
    }
}

class Role extends Model
{
    public function users()
    {
        return $this->belongsToMany('App\User');
    }
} 
```

## Querying Relationships - Multiple Levels

- [Laravel – querying any level far relations with simple trick](http://softonsofa.com/laravel-querying-any-level-far-relations-with-simple-trick/)
- queries any level of nesting and any type of relation

```php
$user->load(['organizations.properties' => function ($q) use ( &$properties ) {
   $properties = $q->get()->unique();
}]);

$properties; // the collection we needed
```
