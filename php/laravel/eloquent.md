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

## Inserting Related Models

- save a comment to a post

```php
$comment = new Comment(['message' => 'A new comment.']);
$post = Post::find(1);
$post->comments()->save($comment);
```

- save multiple comments to a post

```php
$post = Post::find(1);

$post->comments()->saveMany([
    new Comment(['message' => 'A new comment.']),
    new Comment(['message' => 'Another comment.']),
]);
```

- when saving a many-to-many relationship, additional paramaters can be addeded to the  intermediate

```php
User::find(1)->roles()->save($role, ['expires' => $expires]);
```

- create a model from data and associate with another model

```php
$post = Post::find(1);

$comment = $post->comments()->create([
    'message' => 'A new comment.',
]);
```

- updating a "belongs to" relationship, setting the foreign key on a child model

```php
$account = App\Account::find(10);
$user->account()->associate($account);
$user->save();
```

- removing a belongsTo relationship
- resets the foreign key and the relation on the child model

```php
$user->account()->dissociate();
$user->save();
```

@ todo many to many

## Querying Relationships - Multiple Levels

- [Laravel – querying any level far relations with simple trick](http://softonsofa.com/laravel-querying-any-level-far-relations-with-simple-trick/)
- queries any level of nesting and any type of relation

```php
$user->load(['organizations.properties' => function ($q) use ( &$properties ) {
   $properties = $q->get()->unique();
}]);

$properties; // the collection we needed
```
