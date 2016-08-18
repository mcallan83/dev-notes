# ES6 Notes

## Variables

### var

- declares a variable, optionally initializing it to a value
- variable declarations, wherever they occur, are processed before any code is executed
- assigning a value to an undeclared variable implicitly creates it as a global variable
- it is recommended to always declare variables, regardless of whether they are in a function or global scope
- declared variables are constrained in the execution context in which they are declared; undeclared variables are always global

```js
function x() {
  y = 1;        // throws error
  var z = 2;
}

x();

console.log(y); // 1
console.log(z); // throws error
```

- declared variables are created before any code is executed

```js
console.log(a);                // throws error
console.log('still going...'); // never executes
```

```js
var a;
console.log(a);                // undefined or ""
console.log('still going...'); // "still going..."
```

- variables appearing to be implicit globals may be references to variables in an outer function scope:

```js
var x = 0;              // declared global

console.log(typeof z);  // undefined (`z` doesn't exist)

function a() {
  var y = 2;            // declared local to function a

  console.log(x, y);    // 0 2 

  function b() {
    x = 3;              // assigns existing global `x`
    y = 4;              // assigns to existing outer `y`
    z = 5;              // creates new global variable `z`
  }

  b();
  console.log(x, y, z); // 3 4 5
}

a();                   // calling a also calls b

console.log(x, z);     // 3 5
console.log(y);        // throws error
```

- [mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)

### let

- declares a block scope local variable, optionally initializing it to a value
- block is defined by opening and closing curly bracket
- aren't hoisted
- cannot redeclare twice in same scope


```js
var a = 1;
var b = 2;

if (a === 1) {
    var a = 11;         // the scope is global
    let b = 22;         // the scope is inside the if-block

    console.log(a);     // 11
    console.log(b);     // 22
} 

console.log(a);         // 11
console.log(b);         // 2
```

- [caniuse](http://caniuse.com/#feat=let)
- [mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)

### const

- create read-only refernce to a value
- block scoped like `let`
- initializer for constant is required - value must be specified in same statement where declared

```js

const AGE;              // throws an error; missing initializer

const AGE = 10;
console.log(AGE);       // 10

const AGE = 20;         // throws error
let AGE = 20;           // throws error
var AGE = 20;           // throws error
AGE = 30;               // throws error

if (AGE == 10) {
    const AGE = 20;     // ok here because scoped to `if` block
    console.log(AGE);   //20
}

console.log(AGE);       // 10

const FOO;          
```

- value is not immutable, but variable identifier cannot be reassigned

```js
const PERSON = {name: 'Paul'};

PERSON = {name: 'Bob'}; // throws error

PERSON.name = 'Jake';   // object keys are not projected, so this works
console.log(PERSON);    // {name: 'Jake'}
```

- [caniuse](http://caniuse.com/#feat=const)
- [mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)


# using var, let, and const

- use `const` by default
- use `let` if rebinding is needed
- don't use `var`in ES6


## Todo

- `Object.freeze()`
