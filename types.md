## Types & Coercion

There are 7 built-in types: 
_null, 
undefined, 
boolean, 
number, 
string, 
object, 
symbol_ (ES6).

All of these are types are called primitives, except for _object_.
```javascript
typeof 0              // number
typeof true           // boolean
typeof 'Hello'        // string
typeof Math           // object
typeof null           // object  !!
typeof Symbol('Hi')   // symbol (New ES6)
```

| Type        | typeof           | Primitive?     |
| ----------- |:----------------:| --------------:|
| Null        | object           | yes            |
| Undefined   | undefined        | yes            |
| Boolean     | boolean          | yes            |
| String      | string           | yes            |
| Number      | number           | yes            |
| Object      | object           | no --an object |
| Function    | function         | no --an object |
| Array       | object           | no --an object |
| Symbol      | symbol           | no --an object |
| []          | object           | no --an object |
| {}          | object           | no --an object |

### Null vs. Undefined

**Undefined** is the absence of a definition. It is used as the default value for uninitialized variables, function arguments that were not provided and missing properties of objects. Functions return _undefined_ when nothing has been explicitly returned.

**Null** is the absence of a value. It is an assignment value that can be assigned to a variable as a representation of ‘no-value’.

Falsy values: _"", 0, null, undefined, NaN, false_.

Anything not explicitly on the falsy list is truthy — *boolean coerced to true*.
```javascript
Boolean(null)         // false
Boolean('hello')      // true 
Boolean('0')          // true 
Boolean(' ')          // true 
Boolean([])           // true 
Boolean(function(){}) // true
```

### String & Number coercion
The first thing you need to be aware of is the `+` operator. This is a tricky operator because it works for both number addition and string concatenation.

But, the `*, /`  and `-` operators are exclusive for numeric operations. When these operators are used with a string, it forces the string to be coerced to a number.

```javascript
1 + "2" = "12"
"" + 1 + 0 = "10"
"" - 1 + 0 = -1
"-9\n" + 5 = "-9\n5"
"-9\n" - 5 = -14
"2" * "3" = 6
4 + 5 + "px" = "9px"
"$" + 4 + 5 = "$45"
"4" - 2 = 2
"4px" - 2 = NaN
null + 1 = 1
undefined + 1 = NaN
```

### == vs. ===
It is widely spread that == checks for equality and === checks for equality and type. Well, that is a misconception.

In fact, == checks for equality with coercion and === checks for equality without coercion — strict equality.

```javascript
2 == '2'            // True
2 === '2'           // False
undefined == null   // True
undefined === null  // False
```

For a cheat sheet, you can click [here](http://dorey.github.io/JavaScript-Equality-Table/).

```javascript
false == ""  // true
false == []  // true
false == {}  // false
"" == 0      // true
"" == []     // true
"" == {}     // false
0 == []      // true
0 == {}      // false
0 == null    // false
```

### Value vs. Reference
Simple values (also known as primitives) are always assigned by value-copy: null, undefined , boolean, number, string and ES6 symbol.

Compound values always create a copy of the reference on assignment: objects, which includes arrays, and functions.
```javascript
var a = 2;        // 'a' hold a copy of the value 2.
var b = a;        // 'b' is always a copy of the value in 'a'
b++;
console.log(a);   // 2
console.log(b);   // 3
var c = [1,2,3];
var d = c;        // 'd' is a reference to the shared value
d.push( 4 );      // Mutates the referenced value (object)
console.log(c);   // [1,2,3,4]
console.log(d);   // [1,2,3,4]
/* Compound values are equal by reference */
var e = [1,2,3,4];
console.log(c === d);  // true
console.log(c === e);  // false
```
To copy a compound value by value, you need to make a copy of it. The reference does not point to the original value.
```javascript
const copy = c.slice()    // 'copy' references to a new value
console.log(c);           // [1,2,3,4]
console.log(copy);        // [1,2,3,4]
console.log(c === copy);  // false
```

### Scope
Scope refers to the execution context. It defines the accessibility of variables and functions in the code.

**Global Scope** is the outermost scope. Variables declared outside a function are in the global scope and can be accessed in any other scope. In a browser, the window object is the global scope.

**Local Scope** is a scope nested inside another function scope. Variables declared in a local scope are accessible within this scope as well as in any inner scopes.

```javascript
function outer() {
  let a = 1;
  function inner() {
    let b = 2;
    function innermost() {
      let c = 3;
      console.log(a, b, c);   // 1 2 3
    }
    innermost();
    console.log(a, b);        // 1 2 — 'c' is not defined
  }
  inner();
  console.log(a);             // 1 — 'b' and 'c' are not defined
}
outer();
```

You may think of Scopes as a series of doors decreasing in size (from biggest to smallest). A short person that fits through the smallest door — **innermost scope** — also fits through any bigger doors — **outer scopes**.

A tall person that gets stuck on the third door, for example, will have access to all previous doors — **outer scopes** — but not any further doors — **inner scopes**.

### Hoisting
The behavior of “moving” var and function declarations to the top of their respective scopes during the compilation phase is called **hoisting**.

Function declarations are completely hoisted. This means that a declared function can be called before it is defined.

```javascript
console.log(toSquare(3));  // 9

function toSquare(n){
  return n*n;
}
```

Variables are partially hoisted. _var_ declarations are hoisted but not its assignments.
_let_ and _const_ are not hoisted.

```javascript
{  /* Original code */
  console.log(i);  // undefined
  var i = 10
  console.log(i);  // 10
}

{  /* Compilation phase */
  var i;
  console.log(i);  // undefined
  i = 10
  console.log(i);  // 10
}
// ES6 let & const
{
  console.log(i);  // ReferenceError: i is not defined
  const i = 10
  console.log(i);  // 10
}
{
  console.log(i);  // ReferenceError: i is not defined
  let i = 10
  console.log(i);  // 10
}
```

### Function Expression vs. Function Declaration
##### Function Expression
A Function Expression is created when the execution reaches it and is usable from then on — it is not hoisted.
```javascript
var sum = function(a, b) {
  return a + b;
}
```

##### Function Declaration
A Function Declaration can be called both before and after it was defined — it is hoisted.

```javascript
function sum(a, b) {
  return a + b;
}
```

### Variables: var, let and const
Before ES6, it was only possible to declare a variable using _var_. Variables and functions declared inside another function cannot be accessed by any of the enclosing scopes — they are function-scoped.

Variables declared inside a block-scope, such as _if_ statements and _for_ loops, can be accessed from outside of the opening and closing curly braces of the block.

Note: An undeclared variable — assignment without _var, let_ or _const_ — creates a _var_ variable in global scope.

```javascript
function greeting() {
  console.log(s) // undefined
  if(true) {
    var s = 'Hi';
    undeclaredVar = 'I am automatically created in global scope';
  }
  console.log(s) // 'Hi'
}
console.log(s);  // Error — ReferenceError: s is not defined
greeting();
console.log(undeclaredVar) // 'I am automatically created in global scope'
```

ES6 _let_ and _const_ are new. They are not hoisted and block-scoped alternatives for variable declaration. This means that a pair of curly braces define a scope in which variables declared with either let or const are confined in.

```javascript
let g1 = 'global 1'
let g2 = 'global 2'
{   /* Creating a new block scope */
  g1 = 'new global 1'
  let g2 = 'local global 2'
  console.log(g1)   // 'new global 1'
  console.log(g2)   // 'local global 2'
  console.log(g3)   // ReferenceError: g3 is not defined
  let g3 = 'I am not hoisted';
}
console.log(g1)    // 'new global 1'
console.log(g2)    // 'global 2'
```

A common misconception is that const is immutable. It cannot be reassigned, but its properties can be **changed!**

```javascript
const tryMe = 'initial assignment';
tryMe = 'this has been reassigned';  // TypeError: Assignment to constant variable.
// You cannot reassign but you can change it…
const array = ['Ted', 'is', 'awesome!'];
array[0] = 'Barney';
array[3] = 'Suit up!';
console.log(array);     // [“Barney”, “is”, “awesome!”, “Suit up!”]
const airplane = {};
airplane.wings = 2;
airplane.passengers = 200;
console.log(airplane);   // {passengers: 200, wings: 2}
```

### Closure
A **closure** is the combination of a function and the lexical environment from which it was declared. Closure allows a function to access variables from an enclosing scope — **environment** — even after it leaves the scope in which it was declared.

```javascript
function sayHi(name){
  var message = `Hi ${name}!`;
  function greeting() {
    console.log(message)
  }
  return greeting
}
var sayHiToJon = sayHi('Jon');
console.log(sayHiToJon)     // ƒ() { console.log(message) }
console.log(sayHiToJon())   // 'Hi Jon!'
```

The above example covers the two things you need to know about closures:

1. Refers to variables in outer scope.
The returned function access the _message_ variable from the enclosing scope.
2. It can refer to outer scope variables even after the outer function has returned. 
_sayHiToJon_ is a reference to the _greeting_ function, created when _sayHi_ was run. The _greeting_ function maintains a reference to its outer scope — **environment** — in which _message_ exists.
One of the main benefits of closures is that it allows **data encapsulation**. This refers to the idea that some data should not be directly exposed. The following example illustrates that.

By the time _elementary_ is created, the outer function has already returned. This means that the _staff_ variable only exists inside the closure and it cannot be accessed otherwise.

```javascript
function SpringfieldSchool() {
  let staff = ['Seymour Skinner', 'Edna Krabappel'];
  return {
    getStaff: function() { console.log(staff) },
    addStaff: function(name) { staff.push(name) }
  }
}

let elementary = SpringfieldSchool()
console.log(elementary)        // { getStaff: ƒ, addStaff: ƒ }
console.log(staff)             // ReferenceError: staff is not defined
/* Closure allows access to the staff variable */
elementary.getStaff()          // ["Seymour Skinner", "Edna Krabappel"]
elementary.addStaff('Otto Mann')
elementary.getStaff()          // ["Seymour Skinner", "Edna Krabappel", "Otto Mann"]
```

Let’s go deeper into closures by solving one of the most common interview problems on this subject:
What is wrong with the following code and how would you fix it?


```javascript
const arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
  setTimeout(function() {
    console.log(`The value ${arr[i]} is at index: ${i}`);
  }, (i+1) * 1000);
}
```

Considering the above code, the console will display four identical messages _"The value undefined is at index: 4"_. This happens because each function executed within the loop will be executed after the whole loop has completed, referencing to the last value stored in _i_, which was 4.

This problem can be solved by using IIFE, which creates a unique scope for each iteration and storing each value within its scope.
```javascript
const arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(`The value ${arr[j]} is at index: ${j}`);
    }, j * 1000);
  })(i)
}
```
Another solution would be declaring the _i_ variable with _let_, which creates the same result.

```javascript
const arr = [10, 12, 15, 21];
for (let i = 0; i < arr.length; i++) {
  setTimeout(function() {
    console.log(`The value ${arr[i]} is at index: ${i}`);
  }, (i) * 1000);
}
```

### Immediate Invoked Function Expression (IIFE)
An IIFE is a function expression that is called immediately after you define it. It is usually used when you want to create a new variable scope.

The **(surrounding parenthesis)** prevents from treating it as a function declaration.

The **final parenthesis()** are executing the function expression.

On IIFE you are calling the function exactly when you are defining it.

```javascript
var result = [];
for (var i=0; i < 5; i++) {
  result.push( function() { return i } );
}
console.log( result[1]() ); // 5
console.log( result[3]() ); // 5
result = [];
for (var i=0; i < 5; i++) {
  (function () {
    var j = i; // copy current value of i
    result.push( function() { return j } );
  })();
}
console.log( result[1]() ); // 1
console.log( result[3]() ); // 3
```
Using IIFE:

* Enables you to attach private data to a function.
* Creates fresh environments.
* Avoids polluting the global namespace.

### Context
**Context** is often confused as the same thing as Scope. To clear things up, lets keep the following in mind:
**Context** is most often determined by how a function is invoked. It always refers to the value of _this_ in a particular part of your code.
**Scope** refers to the visibility of variables.

### Function calls: call, apply and bind
All of these three methods are used to attach _this_ into function and the difference is in the function invocation.

_.call()_ invokes the function immediately and requires you to pass in arguments as a list (one by one).

_.apply()_ invokes the function immediately and allows you to pass in arguments as an array.

_.call()_ and _.apply()_ are mostly equivalent and are used to borrow a method from an object. Choosing which one to use depends on which one is easier to pass the arguments in. Just decide whether it’s easier to pass in an array or a comma separated list of arguments.

_Quick tip: Apply for Array — Call for Comma._

```javascript
const Snow = {surename: 'Snow'}
const char = {
  surename: 'Stark',
  knows: function(arg, name) {
    console.log(`You know ${arg}, ${name} ${this.surename}`);
  }
}
char.knows('something', 'Bran');              // You know something, Bran Stark
char.knows.call(Snow, 'nothing', 'Jon');      // You know nothing, Jon Snow
char.knows.apply(Snow, ['nothing', 'Jon']);   // You know nothing, Jon Snow
```
**Note:** If you pass in an array as one of the arguments on a call function, it will treat that entire array as a single element. 
ES6 allows us to spread an array as arguments with the call function.

```javascript
char.knows.call(Snow, ...["nothing", "Jon"]);  // You know nothing, Jon Snow
```

_.bind()_ returns a new function, with a certain context and parameters. It is usually used when you want a function to be called later with a certain context.

That is possible thanks to its ability to maintain a given context for calling the original function. This is useful for asynchronous callbacks and events.

_.bind()_ works like the call function. It requires you to pass in the arguments one by one separated by a comma.
```javascript
const Snow = {surename: 'Snow'}
const char = {
  surename: 'Stark',
  knows: function(arg, name) {
    console.log(`You know ${arg}, ${name} ${this.surename}`);}
  }
const whoKnowsNothing = char.knows.bind(Snow, 'nothing');
whoKnowsNothing('Jon');  // You know nothing, Jon Snow
```

### 'this' keyword
Understanding the keyword _this_ in JavaScript, and what it is referring to, can be quite complicated at times.

The value of _this_ is usually determined by a functions execution context. Execution context simply means how a function is called.

The keyword _this_ acts as a placeholder, and will refer to whichever object called that method when the method is actually used.

The following list is the ordered rules for determining this. Stop at the first one that applies:

* _new_ binding — When using the _new_ keyword to call a function, _this_ is the newly constructed object.
```javascript
function Person(name, age) {
  this.name = name;
  this.age =age;
  console.log(this);
}
const Rachel = new Person('Rachel', 30);   // { age: 30, name: 'Rachel' }
```
* **Explicit binding** — When call or apply are used to call a function, _this_ is the object that is passed in as the argument.
Note: _.bind()_ works a little bit differently. It creates a new function that will call the original one with the object that was bound to it.
```javascript
function fn() {
  console.log(this);
}
var agent = {id: '007'};
fn.call(agent);    // { id: '007' }
fn.apply(agent);   // { id: '007' }
var boundFn = fn.bind(agent);
boundFn();         // { id: '007' }
```

* **Implicit binding** — When a function is called with a context (the containing object), _this_ is the object that the function is a property of.
This means that a function is being called as a method.
```javascript
var building = {
  floors: 5,
  printThis: function() {
    console.log(this);
  }
}
building.printThis();  // { floors: 5, printThis: function() {…} }
```

* **Default binding** — If none of the above rules applies, _this_ is the global object (in a browser, it’s the window object).
This happens when a function is called as a standalone function.
A function that is not declared as a method automatically becomes a property of the global object.
```javascript
function printWindow() {
  console.log(this)
}
printWindow();  // window object
```
**Note:** This also happens when a standalone function is called from within an outer function scope.

```javascript
function Dinosaur(name) {
  this.name = name;
  var self = this;
  inner();
  function inner() {
    alert(this);        // window object — the function has overwritten the 'this' context
    console.log(self);  // {name: 'Dino'} — referencing the stored value from the outer context
  }
}
var myDinosaur = new Dinosaur('Dino');
```

* **Lexical this** — When a function is called with an arrow function _=>_, _this_ receives the _this_ value of its surrounding scope at the time it’s created.
_this_ keeps the value from its original context.
```javascript
function Cat(name) {
  this.name = name;
  console.log(this);   // { name: 'Garfield' }
  ( () => console.log(this) )();   // { name: 'Garfield' }
}
var myCat = new Cat('Garfield');
```

### Strict Mode
JavaScript is executed in strict mode by using the _“use strict”_ directive. Strict mode tightens the rules for parsing and error handling on your code.

Some of its benefits are:

* **Makes debugging easier** — Code errors that would otherwise have been ignored will now generate errors, such as assigning to non-writable global or property.
* **Prevents accidental global variables** — Assigning a value to an undeclared variable will now throw an error.
* **Prevents invalid use of delete** — Attempts to delete variables, functions and undeletable properties will now throw an error.
* **Prevents duplicate property names or parameter values** — Duplicated named property in an object or argument in a function will now throw an error. (This is no longer the case in ES6)
* **Makes eval() safer** — Variables and functions declared inside an _eval()_ statement are not created in the surrounding scope.
* **“Secures” JavaScript eliminating this coercion** — Referencing a _this_ value of null or undefined is not coerced to the global object. This means that in browsers it’s no longer possible to reference the window object using _this_ inside a function.

### `new` keyword
The _new_ keyword invokes a function in a special way. Functions invoked using the _new_ keyword are called constructor functions.

So what does the _new_ keyword actually do?

1.Creates a new object.
2. Sets the object’s prototype to be the prototype of the constructor function.
3. Executes the constructor function with this as the newly created object.
4. Returns the created object. If the constructor returns an object, this object is returned.

```javascript
// In order to better understand what happens under the hood, lets build the new keyword 
function myNew(constructor, ...arguments) {
  var obj = {}
  Object.setPrototypeOf(obj, constructor.prototype);
  return constructor.apply(obj, arguments) || obj
}
```
What is the difference between invoking a function with the _new_ keyword and without it?

```javascript
function Bird() {
  this.wings = 2;
}
/* invoking as a normal function */
let fakeBird = Bird();
console.log(fakeBird);    // undefined
/* invoking as a constructor function */
let realBird= new Bird();
console.log(realBird)     // { wings: 2 }
```

### Prototype and Inheritance
Prototype is one of the most confusing concepts in JavaScript and one of the reason for that is because there are two different contexts in which the word **prototype** is used.

* **Prototype relationship**
Each object has a **prototype** object, from which it inherits all of its prototype’s properties.
_.__proto__ _ is a non-standard mechanism (available in ES6) for retrieving the prototype of an object (*). It points to the object’s “parent” — the **object’s prototype**. 
All normal objects also inherit a _.constructor_ property that points to the constructor of the object. Whenever an object is created from a constructor function, the _.__proto__ _ property links that object to the _.prototype_ property of the constructor function used to create it.
_(*) Object.getPrototypeOf()_ is the standard ES5 function for retrieving the prototype of an object.
* **Prototype property** 
Every function has a _.prototype_ property. 
It references to an object used to attach properties that will be inherited by objects further down the prototype chain. This object contains, by default, a _.constructor_ property that points to the original constructor function. 
Every object created with a constructor function inherits a constructor property that points back to that function.
```javascript
function Dog(breed, name){
  this.breed = breed,
  this.name = name
}
Dog.prototype.describe = function() {
  console.log(`${this.name} is a ${this.breed}`)
}
const rusty = new Dog('Beagle', 'Rusty');

/* .prototype property points to an object which has constructor and attached 
properties to be inherited by objects created by this constructor. */
console.log(Dog.prototype)  // { describe: ƒ , constructor: ƒ }

/* Object created from Dog constructor function */
console.log(rusty)   //  { breed: "Beagle", name: "Rusty" }
/* Object inherited properties from constructor function's prototype */
console.log(rusty.describe())   // "Rusty is a Beagle"
/* .__proto__ property points to the .prototype property of the constructor function */ 
console.log(rusty.__proto__)    // { describe: ƒ , constructor: ƒ }
/* .constructor property points to the constructor of the object */
console.log(rusty.constructor)  // ƒ Dog(breed, name) { ... }
```

### Prototype Chain
The prototype chain is a series of links between objects that reference one another.

When looking for a property in an object, JavaScript engine will first try to access that property on the object itself.

If it is not found, the JavaScript engine will look for that property on the object it inherited its properties from — the **object’s prototype**.

The engine will traverse up the chain looking for that property and return the first one it finds.

The last object in the chain is the built-in _Object.prototype_, which has _null_ as its prototype. Once the engine reaches this object, it returns _undefined_.

### Own vs Inherited Properties
Objects have own properties and inherited properties.

Own properties are properties that were defined on the object.

Inherited properties were inherited through prototype chain.

```javascript
function Car() { }
Car.prototype.wheels = 4;
Car.prototype.airbags = 1;

var myCar = new Car();
myCar.color = 'black';

/*  Check for Property including Prototype Chain:  */
console.log('airbags' in myCar)  // true
console.log(myCar.wheels)        // 4
console.log(myCar.year)          // undefined

/*  Check for Own Property:  */
console.log(myCar.hasOwnProperty('airbags'))  // false — Inherited
console.log(myCar.hasOwnProperty('color'))    // true
```

**Object.create(obj)** — Creates a new object with the specified **prototype** object and properties.
```javascript
var dog = { legs: 4 };
var myDog = Object.create(dog);

console.log(myDog.hasOwnProperty('legs'))  // false
console.log(myDog.legs)                    // 4
console.log(myDog.__proto__ === dog)       // true
```

### Inheritance by reference
An inherited property is a copy by reference of the **prototype object’s** property from which it inherited that property.

If an object’s property is mutated on the prototype, objects which inherited that property will share the same mutation. But if the property is replaced, the change will not be shared.

```javascript
var objProt = { text: 'original' };
var objAttachedToProt = Object.create(objProt);
console.log(objAttachedToProt.text)   // original

objProt.text = 'prototype property changed';
console.log(objAttachedToProt.text)   // prototype property changed

objProt = { text: 'replacing property' };
console.log(objAttachedToProt.text)   // prototype property changed
```



```javascript

```
Original: [link](https://medium.freecodecamp.org/the-definitive-javascript-handbook-for-a-developer-interview-44ffc6aeb54e)
