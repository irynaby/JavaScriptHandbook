## Types & Coercion
=======

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

#### Null vs. Undefined

*Undefined* is the absence of a definition. It is used as the default value for uninitialized variables, function arguments that were not provided and missing properties of objects. Functions return _undefined_ when nothing has been explicitly returned.

*Null* is the absence of a value. It is an assignment value that can be assigned to a variable as a representation of ‘no-value’.

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

#### String & Number coercion
The first thing you need to be aware of is the + operator. This is a tricky operator because it works for both number addition and string concatenation.

But, the *, /  and -operators are exclusive for numeric operations. When these operators are used with a string, it forces the string to be coerced to a number.

