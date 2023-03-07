
# What The JavaScript
## Introduction


### Primitives
Simplest primitive is a primitive boolean.
Simple 0 or 1s.

#### Boolean

```javascript
true    // true
false   // false
```

Creating booleans is as simple as:
```javascript
new Boolean()               // false
new Boolean(0)              // false
new Boolean(1)              // true
new Boolean('')             // false
new Boolean('123')          // true
new Boolean({})             // true
new Boolean([])             // true
new Boolean(function(){})   // true
new Boolean(undefined)      // false
new Boolean(undefined)      // false
```

```javascript
typeof true                         // boolean
true instanceof Boolean             // false
typeof Boolean                      // function
Boolean instanceof Function         // true
new Boolean() instanceof Boolean    // true
```

This means we can create new instances of functions.
```javascript
const test = function(){console.log(123)}
test()                      // 123
new test()                  // 123 & test {}
const instance = new test();
instance instanceof test    // true
```

##### Conclusion
Testing for primitive types should always be done using `typeof`, not `instanceof` 
```javascript
 typeof test == 'function' // true
```
Primitive values and object values can be a little confusing in JavaScript because the language implicitly wraps 
primitives in appropriate object wrappers when they are used like object references. That's why you can write
```javascript
true.valueOf()  // true
true.toString() // 'true'

Boolean.prototype.wtf = function(obj) {
    console.log('wtf is this monkey doing');
}
```

#### Type Coercion
Type coercion is the automatic or implicit conversion of values from one data type to another.

```javascript
123 == '123'        // true
123 === '123'       // false
123 === +'123'      // true
123 ===+     '123'  // true
```

Truthy and falsy values are values that are considered true or false when encountered in a Boolean context. 
```javascript
new Boolean({})    // true
{} == true         // false
!!{} == true       // true
!!'123' == true    // true
!!'' == true       // false
```
One would think in a boolean context, everything gets converted to truthy or falsy, then this is happening:
```javascript
{} == {}                // false
{} == []                // false
[] == []                // false
[] == ''                // true
{} == ''                // false
{} == '[object Object]' // true
```
It's simply auto-converting these arrays and object to strings

#### Numbers

```javascript
123     // 123
false   // false
```
+value converts it to a number.

and isNan is odd.
```javascript
+'123'              // 123
+123                // 123
+true               // 1
+false              // 0
+{}                 // NaN
+[]                 // 0
+[1,2,3]            // NaN
+null               // 0
+null === +false;   // true
isNaN([])           // false
isNaN('123')        // false
isNaN({})           // false
isNaN(undefined)    // true
isNaN(null)         // false
isNaN(NaN)          // true
typeof NaN          // number
```
The ECMAScript Language Specification explains NaN as a number value that is a IEEE 754 “Not-a-Number” value. It might seem strange, but this is a common computer science principle.

```javascript
NaN == NaN          // false
Object.is(NaN, NaN) // true
0 === -0;           // true
Object.is(0, -0);   // false
```

Similar to booleans, we can create new numbers
```javascript
new Number(123) == Number(123)      // true
new Number(123) === Number(123)     // false
new Number(123) == new Number(123)  // false
new Number(123).valueOf()           // 123
4.toString();                       // an error
4.20                                // 4.20
4,20                                // 20
mamamia='',lalal=123                // 123
(2).toString();                     // '2'
```

Now some back to school math
```javascript
0420 - 069          // 203    
null + 0            // 0
1 + 2 + "3"         // '33'
1 + 2 + +"3"        // '33'
0/0                 // NaN - according to google, its undefined
1/0                 // Infinity
-1/0                // -Infinity
!5 + !5             // 0
+!!NaN * "" - - [,] // 0 -> 0 * 0 - -0; // -> 0
```

some random number related stuff
```javascript
3 > 2 > 1   // false
3 + "3"     // '33'
3 - "3"     // 0
"" - 1      // -1
```

More real-world scenarios:
```javascript
parseInt(420)           // 420    
parseInt(420.69)        // 420    
parseInt(0.0000005)     // 5   --> 5e-7
parseInt('5e-7')        // 5
```

```javascript
parseInt(Infinity)      // NaN
parseInt(Infinity, 30)  // 13693557269
```
First, Infinity gets converted to the string 'Infinity'. 
Therefore, the first result shouldn't be surprising. 
We are using the radix 30, and in base 30, all letters are valid up to y. 
And 'Infinit' in base 30 is 13693557269 in base 10.


```javascript
parseInt(3)                          // 3
['2','7','11'].map(parseInt)        // [2, NaN, 3]
[1,2,3,4].map(parseInt)             // [1, NaN, NaN, NaN]
['1','2','3','4'].map(Math.floor)   // [1, 2, 3, 4]
['2','7','11'].map(parseInt)
```
`.map` passes two (actually three) arguments to parseInt. 1 - the value, 2 - the index, 3 - the array.
Each item in the array is parsed using a different radix. Because index 0 is false, '1' is parsed with default radix 10,
'7' is parsed with radix 1, which is unparseable and '11' is parsed with radix 2, so the result is 3.

```javascript
x.toFixed(2)    // '123.70'
+x.toFixed(2)   // 123.7
```

Safe numbers
```javascript
Number.MAX_SAFE_INTEGER                                     // 9007199254740991 --> 2^53-1
Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2 // true

function doSomething(id) {
    console.log(id);
}

doSomething(9007199254740992); // logs 9007199254740992
doSomething(9007199254740993); // logs 9007199254740992
doSomething(9007199254740994); // logs 9007199254740994
```
To avoid these issues, use libraries that support arbitrary-precision arithmetic, 
such as `BigInt` in newer versions of JavaScript, or use string representations of large numbers instead.


#### String

```javascript
(69).toString()             // '69'
(69).toString(2)            // '1000101'
69 + ''                     // '69'

'b' + 'a' + 'a' + 'a'       // 'baa'
'b' + 'a' + + 'a' + 'a'     // 'baNaNa'

string.replace
string.replaceAll // dont fall for it
const string = 'e851e2fa-4f00-4609-9dd2-9b3794c59619'
console.log(string.replace('-', ''))    // e851e2fa4f00-4609-9dd2-9b3794c59619

"" - - "" // These two empty strings are both converted to 0.

Number(""); // -> 0
0 - - 0; // -> 0
// The expression might become a bit clearer if I write it like this:

+"" - -""   // 0
+0 - -0     // 0

- -"";  // 0
--"";   // SyntaxError, space is important
```
