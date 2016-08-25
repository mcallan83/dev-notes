# ES6 Notes

- [ECMAScript 6 Compatibility Table](http://kangax.github.io/compat-table/es6/)
- [ES6 In Depth](https://ponyfoo.com/articles/tagged/es6-in-depth)
- [ES6 Overview in 350 Bullet Points](https://ponyfoo.com/articles/es6)

## Let, Const, and Var

- `let` and `const` are alternatives to `var` when declaring variables
- [ES6 Let, Const and the “Temporal Dead Zone” (TDZ) in Depth](https://ponyfoo.com/articles/es6-let-const-and-temporal-dead-zone-in-depth)

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
console.log('still going...'); // "still going..."

```

```js
console.log(typeof a);         // undefined
console.log(a);                // undefined
console.log('still going...'); // "still going..."
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
    console.log(typeof value); // "undefined"
    console.log(value); // undefined
    var value = 3;
}

// implicitly understood as:

var value;

function test () {
    var value;
    console.log(typeof value); // "undefined"
    console.log(value); // undefined
    value = 3;
}

value = 2;

test();

```


- [MDN: var](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)
- [MDN: hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)
- [JavaScript Variable Hoisting](https://ponyfoo.com/articles/javascript-variable-hoisting)

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

- [Can I Use: let](http://caniuse.com/#feat=let)
- [MDN: let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)

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

- [Can I Use: const](http://caniuse.com/#feat=const)
- [MDN: const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

### using var, let, and const

- use `const` by default
- use `let` if rebinding is needed
- don't use `var`in ES6

## Arrow Functions

- more consise over regular functions
- also called "fat arrow" functions

```js

const names = ['jack', 'joe', 'james'];

// es5 way
const fullNameES5 = names.map(function(name) {
    return name + " smith";
});

// es6 way
const fullNamesES6 = names.map((name) => {
    return `${name} smith`;
});

```

- are always anonmous functions, but can be assigned to a variable

```js

const sayHello = (name) => { console.log(`Hello ${name}`) }
sayHello('Mike'); //Hello Mike

```

### Parameters

- parenthesis can be dropped if there is only 1 parameter

```js

const names = ['jack', 'joe', 'james'];

const fullNames = names.map(name => {
    return `${name} smith`;
});

```

- parenthesis are required if there are zero or more than 1 parameter

```js

const names = ['jack', 'joe', 'james'];

const zeroParams = names.map(() => {
    return `name`;
});

const twoParams = names.map((name, index) => {
    return index;
});
```

### Implicit Returns

- concise body functions allow for a single expression without brackets, while attaching an implicit return

```js

const names = ['jack', 'joe', 'james'];

const blockBody = names.map(name => {
    return index;
});

// is equivilent to:

const conciseBody = names.map(name => `${name} smith`);

```

- when returning an object literal expression, the body needs to be wrapped in parentheses to distinguish between a block and an object

```js

const names = ['jack', 'joe', 'james'];

const nameData = names.map((name,index) => ({name: name, id: index}));

```








----


- doesn't rebind value of `this` when using arrow function inside of another function


- {MDN: Arrow Functions}(https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
