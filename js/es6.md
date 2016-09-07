ES6 Notes
=========

- [ECMAScript 6 Compatibility Table](http://kangax.github.io/compat-table/es6/)
- [ES6 In Depth](https://ponyfoo.com/articles/tagged/es6-in-depth)
- [ES6 Overview in 350 Bullet Points](https://ponyfoo.com/articles/es6)

Let, Const, and Var
-------------------

- `let` and `const` are alternatives to `var` when declaring variables

### var

- declares a variable, optionally initializing it to a value
- processed before any code is executed
- assigning a value to an undeclared variable (not using `var`) creates a global variable; recommended to always declare variables
- are lexically scoped to a funciton

```js

function x() {
  y = 1;
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
console.log(a);                // undefined
console.log('still going...'); // 'still going...'

```

```js
console.log(typeof a);         // undefined
console.log(a);                // undefined
console.log('still going...'); // 'still going...'
var a;
```

- variables appearing to be implicit globals may be references to variables in an outer function scope

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

a();                    // calling a also calls b

console.log(x, z);      // 3 5
console.log(y);         // throws error

```

- declared variables are hoisted to the top of a function
  + moves variable declarations to the top of the scope
  + assignments stay where they are
  + function declarations are hoisted only if not part of an assignment statement

```js

foo = 2
var foo;

// implicitly understood as:

var foo;
foo = 2;

```

```js

var value = 2;

test();

function test () {
    console.log(typeof value);  // undefined
    console.log(value);         // undefined
    var value = 3;
}

// implicitly understood as:

var value;

function test () {
    var value;
    console.log(typeof value);  // undefined
    console.log(value);         // undefined
    value = 3;
}

value = 2;

test();

```

### let

- declares a block scope local variable, optionally initializing it to a value
- block is defined by opening `{` and closing  `}` curly bracket
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

### using var, let, and const

- use `const` by default
- use `let` if rebinding is needed
- don't use `var`in ES6

### links

- [CIU: const](http://caniuse.com/#feat=const)
- [CIU: let](http://caniuse.com/#feat=let)
- [ES6 Let, Const and the "Temporal Dead Zone" (TDZ) in Depth](https://ponyfoo.com/articles/es6-let-const-and-temporal-dead-zone-in-depth)
- [JavaScript Variable Hoisting](https://ponyfoo.com/articles/javascript-variable-hoisting)
- [MDN: const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
- [MDN: hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)
- [MDN: let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
- [MDN: var](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)

Arrow Functions
---------------

- more consise over regular functions
- also called "fat arrow" functions
- always anonymous functions, but can be assigned to a variable

```js

const names = ['jack', 'joe', 'james'];

// es5 way
const fullNameES5 = names.map(function(name) {
    return name + " smith";
});
console.log(fullNameES5);     //["jack smith", "joe smith", "james smith"]

// es6 way
const fullNamesES6 = names.map((name) => {
    return `${name} smith`;
});
console.log(fullNamesES6);    //["jack smith", "joe smith", "james smith"]

```

- parenthesis can be dropped if there is only 1 parameter, but are required for zero or more than one paramaters

```js

() => { ... }       // no parameter
x => { ... }        // one parameter, an identifier
(x, y) => { ... }   // multiple parameters

```

- expression body functions allow for a single expression without brackets, while attaching an implicit return

```js

const names = ['jack', 'joe', 'james'];

const blockBody = names.map(name => {
    return index;
});

// equivilent to:

const expressionBody = names.map(name => `${name} smith`);

```

- when returning an object literal expression, the body needs to be wrapped in parentheses to distinguish between a block and an object

```js

const names = ['jack', 'joe', 'james'];

const nameData = names.map((name,index) => ({name: name, id: index}));

```

- value of `this` is determined by the surrounding scope (lexical), meaning it has the same context as `this` in the parent scope
- `this` can't be modified with `.call` or `.apply`

### links

- [MDN: Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [ES6 Arrow Functions in Depth](https://ponyfoo.com/articles/es6-arrow-functions-in-depth)


Default Function Paramaters
---------------------------

- parameters default to `undefined` when a value is not passed to a function, unless default values are provided
- if a value of `underfined` is explicitly passed to a function, the function will use a default value, if provided

```js

function createUser(name, sex = 'male', income) {
  return [name, gender, income];
}

createUser('john');                   // ['john', 'male', undefined]
createUser('john', undefined);        // ['john', 'male', undefined]
createUser('jane', 'female', 30000);  // ['jane', 'female', 30000]

```

- default parameters are available to later default parameters

```js

function getFullName(first, last, full = first + ' ' + last) {
  return full;
}

console.log(getFullName('mike', 'smith'));              // 'mike smith'
console.log(getFullName('mike', 'smith', 'full name')); // 'full name'

```

### links

[MDN: Default Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)
