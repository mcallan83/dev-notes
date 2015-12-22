# PHP / String Functions

- `str_replace($search, $replace, $subject)`
    - returns a string or an array with all occurrences of _search_ in _subject_ replaced with the given _replace_ value

```php
<?php
$search = "###";
$replace = "black";
$subject = "<body text=###>";

$result = str_replace($search, $replace, $subject)

// <body text=black>
?>
```
