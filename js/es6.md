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

### when not to use arrow functions

- you really need `this`

```js

const button = document.querySelector('#button');

// bad
button.addEventListener('click', () => {
  console.log(this);                              \\ Window
  this.classList.toggle('on');                    \\ throws error
});

// good
button.addEventListener('click', function() {
  console.log(this);                              \\ button
  this.classList.toggle('on');                    \\ toggles
});

```

- you need a method to bind to an object

```js

// bad
let person = {
  points: 20,
  score: () => {
    this.points++;
  }
}
person.score(); // does not work since `this` is not person

// good
person= {
  points: 20,
  score: function() {
    this.points++;
  }
}
person.score(); // increments person.points by 1

```

- when you need to add a prototype method

```js

class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

const joe = new Person('Joe', 33);

// bad
Person.prototype.greet = () => {
  return `Hello ${this.name}`
}
console.log(joe.greet()) // "Hello undefined"

// good
Person.prototype.greet = function() {
  return `Hello ${this.name}`
}
console.log(joe.greet()) // "Hello Joe"

```

- when you need access to the arguments object

```js

// bad 
const logArray = () => {
  console.log(Array.from(arguments)); 
}
logArray(1,2,3) // throws error

// good 
const logArray = function() {
  console.log(Array.from(arguments));
}
logArray(1,2,3) // [1, 2, 3]

```


### links

- [MDN: Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [ES6 Arrow Functions in Depth](https://ponyfoo.com/articles/es6-arrow-functions-in-depth)

Default Function Paramaters
---------------------------

- parameters default to `undefined` when a value is not passed to a function, unless default values are provided
- if a value of `underfined` is explicitly passed to a function, the function will use a default value, if provided

```js

function createUser(name, gender = 'male', income) {
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

Template Literals
-----------------

- utilizes backticks as opposed to single or double quotes
- interolates variables or expressions into strings

```js

const name = 'Spot';
const age = 3;

const sentance = `My dog ${name} is ${age * 7} years old.`

console.log(sentance); // My dog Spot is 21 years old.

```

- supports multiple lines

```js

const dog = `
    <div class="dog">
      <h2>${name}</h2>
    </div>
`;

```

- can be nested

```js

const employees = [
  { name: 'John', age: 25 },
  { name: 'Jack', age: 30 }
];

const html = `
  <ul class="employees">
    ${employees.map(person => `
      <li>
        ${person.name}
        ish
        ${person.age}
      </li>
    `).join('')}
  </ul>
`;

```

### tagged template literals

- tagged template literals use a custom method to render a template
- tag a template by placing word before opening backtick (`)that matches method name

```js

// simulates "normal" behavor for template rendering
function render (template, ...expressions) {
  return template.reduce((accumulator, part, i) => {
    return accumulator + expressions[i - 1] + part
  })
}

// capitalize all expressions
function upperExpr (template, ...expressions) {
  return template.reduce((accumulator, part, i) => {
    return accumulator + expressions[i - 1].toUpperCase() + part
  })
}

const name = 'jack'
const car = 'honda civic'

const text = render`hello ${name}, you ${car} is very shinny`
const altText = upperExpr`hello ${name}, you ${car} is very shinny`

console.log(text)     // hello jack, you honda civic is very shinny
console.log(altText)  // hello JACK, you HONDA CIVIC is very shinny
```

### links

- [ES6 Template Literals in Depth](https://ponyfoo.com/articles/es6-template-strings-in-depth)
- [Demystifying Tagged Templates](https://ponyfoo.com/articles/es6-template-strings-in-depth#demystifying-tagged-templates)
- [MDN: Template Literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
- [Template String Compatibility](http://kangax.github.io/compat-table/es6/#test-template_strings)

New String Methods
------------------

.startsWith()

.endsWith()

.includes()

.repeat()

//@todo

Destructuring
-------------

- extract data from arrays, objects, maps, and sets into distinct variables

### objects

```js

const person = {
  first: 'Jack',
  last: 'Smith',
  country: 'US',
  city: 'Chicago',
  links: {
    social: {
      'twitter': 'http://twitter.com/jsmith',
      'facebook': 'http://facebook.com/jsmith'
    }
  }
}

const {first, last} = person;
const {twitter, facebook} = person.links.social;

console.log(first)    // 'Jack'
console.log(last)     // 'Smith'

console.log(twitter)  // 'http://twitter.com/jsmith'
console.log(facebook) // 'http://facebook.com/jsmith'

```

- variables can be renamed as they are extracted

```js

const person = {
  first: 'Jack',
  last: 'Smith',
  country: 'US',
  city: 'Chicago',
  links: {
    social: {
      'twitter': 'http://twitter.com/jsmith',
      'facebook': 'http://facebook.com/jsmith'
    }
  }
}

const {facebook: fb} =  person.links.social;

console.log(fb) // 'http://facebook.com/jsmith'

```

- defaults can be provided for variables being extracted in case their values are undefined

```js

const settings = {width: 300, color: 'black'}
const {width = 100, height = 100, color = 'blue', fontSize = 20} = settings

console.log(width)      // 300
console.log(height)     // 100
console.log(color)      // 'black'
console.log(fontSize)   // 20

```

- complex example

```js

const {w: width = 400, h: height = 500} = {w: 800}

console.log(width); // 800
console.log(height); // 500

```

### arrays

```js

const person = ['Jack Smith', 32, 'jack@gmail.com'];

// destructure first two elements in array
const [name, age] = person;

console.log(name)   // Jack Smith
console.log(age)    // 32

```

- arrays of unknown length can be destructured

```js

const team = ['Jack', 'John', 'James', 'Jane', 'Juan'];

// rest operator grabs the "rest" of the array
const [captain, assistant, ...players] = team;

console.log(captain)    // Jack
console.log(assistant)  // Jane
console.log(players)    // ['James', 'Jane', 'Juan']

```

- the values of two variables can be swapped

```js

let inRing = 'Hulk Hogan'
let onSide = 'The Rock'

[inRing, onSide] = [onSide, inRing];

console.log(inRing) // 'The Rock'
console.log(onSide) // 'Hulk Hogan'

```

- defaults can be provided for variables being extracted in case their values are undefined

```js

const [a = 5, b = 7] = [1];

console.log(a) // 1
console.log(b) // 7

```

### functions

- allows the returning of multiple values from a function (sort of)

```js

function convertCurrency(amount) {
  return {
    USD: amount * .75,
    MEX: amount * 13.30,
    AUD: amount * 1.01
  }
}

const {USD, AUD} = convertCurrency(100);

console.log(USD) // 75
console.log(AUD) // 101

```

- can ignore some return values
 
```js

function f() {
  return [1, 2, 3];
}

var [a, , b] = f();

console.log(a); // 1
console.log(b); // 3

```

- fields can be pulled from objects passed as a funciton parameter

```js

function getUserId({id}) {
  return id;
}

const user = {
  id: 100,
  name: 'Jack'
}

console.log("ID: " + getUserId(user)); // ID: 100

```

### links

- [Destructing assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
- [ES6 JavaScript Destructuring in Depth](https://ponyfoo.com/articles/es6-destructuring-in-depth)

Iterables & Looping
-------------------

- iterable - anything that can be looped over (array, string, map, set, etc...)

### for

- consists of three optional expressions, enclosed in parentheses and separated by semicolons
- complicated syntax

```js

const animals = ['Cat', 'Dog', 'Fish', 'Turtle'];

for (let i = 0; i < animals.length; i++) {
  console.log(animals[i]);
}

// Cat
// Dog
// Fish
// Turtle

```

### for each

- simpler syntax
- cannot use [break](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/break) or [continue](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/continue) within loop body
- executes a provided function once per array element

```js

const animals = ['Cat', 'Dog', 'Fish', 'Turtle'];

animals.forEach((animal) => console.log(animal));

// Cat
// Dog
// Fish
// Turtle

```

### for in

- iterates over all enumerable properties of an object (even a method added to the prototype) in arbitrary order
- loops in in arbitrary order

```js

const animals = ['Cat', 'Dog', 'Fish', 'Turtle'];

animals.test = 1234;

for (const index in animals) {
  console.log(animals[index]);
}

// Cat
// Dog
// Fish
// Turtle
// 1234

```

- use [hasOwnProperty()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty) to skip inherited properties

```js

var triangle = {a:1, b:2, c:3};

function ColoredTriangle() {
  this.color = "red";
}

ColoredTriangle.prototype = triangle;

var obj = new ColoredTriangle();

for (var prop in obj) {
  if( obj.hasOwnProperty( prop ) ) {
    console.log("obj." + prop + " = " + obj[prop]);
  } 
}

// "obj.color = red"

```

### for of

- can't use with objects (directly)
- syntax is specific to collections, rather than all objects
- will iterate over the elements of any collection that has a `Symbol.iterator` property

```js

const animals = ['Cat', 'Dog', 'Fish', 'Turtle'];

animals.text = 1234;

for(const animal of animals) {
  console.log(animal)
}

// Cat
// Dog
// Fish
// Turtle

```

- able to use break and continue

```js

const animals = ['Cat', 'Dog', 'Fish', 'Turtle'];

// stop after "Dog"
for(const animal of animals) {
  console.log(animal)
  if(animal == "Dog") {
    break;
  }
}

// Cat
// Dog


// skip "Dog"
for(const animal of animals) {
  if(animal == "Dog") {
    continue;
  }
  console.log(animal);
}

// Cat
// Fish
// Turtle

```

- can loop over an iterator

```js

const animals = ['Cat', 'Dog', 'Fish', 'Turtle'];

for (const animal of animals.entries()) {
    console.log(animal);
}

// [0, "Cat"]
// [1, "Dog"]
// [2, "Fish"]
// [3, "Turtle"] 

// with destructuring
for (const [i, animal] of animals.entries()) {
    console.log(i, animal);
}

// 0 "Cat"
// 1 "Dog"
// 2 "Fish"
// 3 "Turtle"

```

### iterating over objects

```js

const dog = {
  size: 'Small',
  color: 'Black',
  weight: 25
};

for (const prop of Object.keys(dog)) {
  const value = dog[prop];
  console.log(prop, value);
}

// size Small
// color Black
// weight 25

```


### links

- [mdn - for in](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)
- [mdn - for of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)
- [mdn - for](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for)
- [mdn - forEach()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)


Array Methods
-------------

### .from()

- on `Array` object itself, not on prototype
- creates a new `Array` instance from:
  - an array-li9ke object (has length property and indexed elements)
  - or iterable object (have access its elements, such as [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) and [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set))

```html

<div class="people">
  <p>Mike</p>
  <p>Michael</p>
  <p>Mikey</p>
</div>

```

```js

// NodeList, not an array
const people = document.querySelectorAll('.people p');

// throws error because people is not an array
const names = people.map(person => person.textContent);

// convert to array
const peopleArray = Array.from(people);

console.log(peopleArray) // ["Mike", "Michael", "Mikey"]

```

- can take a second argument that is a map function

```html

<div class="people">
  <p>Mike</p>
  <p>Michael</p>
  <p>Mikey</p>
</div>

```

```js

const people = document.querySelectorAll('.people p');

const peopleArray = Array.from(people, person => person.textContent);

console.log(peopleArray) // ["Mike", "Michael", "Mikey"]

```

### .of()

- on `Array` object itself, not on prototype
- creats a new `Array` instance with all passed arguments, reguardless of type

```js

Array.of(1);         // [1]
Array.of(1, 2, 3);   // [1, 2, 3]
Array.of(undefined); // [undefined]

```

### .find()

-  executes the callback function once for each element present in the array until it finds one where callback returns a true value
-  if an element is found, it returns its value, otherwise undefined is returned
- does not mutate the array on which it is called
- [.filter()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) is similar to [.find()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find), but returns multiple results as an array

```js

const classes = [
  {
    "name": "english",
    "id": 1234,
    "level": 1
  },
  {
    "name": "algebra",
    "id": 5678,
    "level": 1
  },
  {
    "name": "history",
    "id": 5432,
    "level": 1
  },
  {
    "name": "algebra",
    "id": 2134,
    "level": 2
  },
];

var historyClass = classes.find(post => post.name === "history");

console.log(historyClass.id) // 5432
```

### .findIndex()

-  executes the callback function once for each element present in the array until it finds one where callback returns a true value
-  if an element is found, it returns its index, otherwise `-1` is returned
- does not mutate the array on which it is called


```js

var historyClass = classes.findIndex(post => post.name === "history");

console.log(historyClass); // 2

```



### links

- [mdn - from()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)
- [mdn - of()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/of)
- [mdn - find()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)
- [mdn - findIndex()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)
- [ES6 Overview: Array](https://ponyfoo.com/articles/es6#array)
