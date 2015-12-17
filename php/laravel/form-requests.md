# PHP / Laravel / Form Requests

## Sanitizing Form Request Data

- [Sanitizing Form Data Before Validating in a Laravel 5 Form Request](http://larabrain.com/tips/sanitizing-form-data-before-validating-in-a-laravel-5-form-request)
- override the `all()` method to sanitize form input
- Laravel will automatically call this method prior to validating

```php
class RegistrationRequest extends Request
{

    public function all()
    {
        $attributes = parent::all();

        $attributes['phone'] = preg_replace("/[^0-9]/", '', $input);

        $this->replace($attributes);

        return parent::all();

    }

}
```
