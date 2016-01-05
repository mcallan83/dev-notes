# PHP - String Functions

- `function str_replace($searchFor, $replaceWith, $source)`
    - returns a string or an array with all occurrences of `searchFor` in `source` replaced with the `replaceWith` value

    ```php
    $search = '###';
    $replace = 'black';
    $subject = '<body text="###">';

    $result = str_replace($searchFor, $replaceWith, $source);
    // '<body text="black">'

    ```
