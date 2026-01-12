# What The JavaScript
JavaScript. The language that everyone loves to hate. Sure, it's powerful and ubiquitous, but it's also one of the most unpredictable and downright weird programming languages out there. From unexpected behavior with arrays and objects, to the infamous "NaN" value and the "truthy/falsy" conundrum, JavaScript is full of quirks and edge cases that can make even experienced developers scratch their heads. We will marvel at the strange and sometimes hilarious things that can happen when you don't know what you're doing. We'll look at real-world examples of JavaScript code gone wrong, and laugh at the sheer absurdity of it all.
## Primitives
Simplest primitive is a primitive boolean.
Simple 0 or 1s.

### Boolean

```javascript
true    // true
false   // false
```

Creating booleans is as simple as:
```javascript
Boolean()               // false
Boolean(0)              // false
Boolean(1)              // true
Boolean('')             // false
Boolean('123')          // true
Boolean({})             // true
Boolean([])             // true
Boolean(function(){})   // true
Boolean(undefined)      // false
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
const t = new test();
t instanceof test           // true
```

#### Conclusion
Testing for primitive types should always be done using `typeof`, not `instanceof` 
```javascript
 typeof test == 'function' // true
```
Primitive values and object values can be a little confusing in JavaScript because the language implicitly wraps 
primitives in appropriate object wrappers when they are used like object references. That's why you can write
```javascript
true.valueOf()  // true
true.toString() // 'true'

Boolean.prototype.wtf = function() {
    console.log('wtf is this monkey doing');
}
true.wtf()
```

### Type Coercion
Type coercion is the automatic or implicit conversion of values from one data type to another.

```javascript
123 == '123'        // true
123 == '00000123'   // true
123 === '123'       // false
123 === +'123'      // true
123 ===+     '123'  // true

null === 0          // false
null == 0           // false
null > 0            // false
null < 0            // false
null >= 0           // true
null <= 0           // true
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

### Numbers

+value converts it to a number.
and isNaN is odd.
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

const res = NaN;
const a = [res];
a.includes(res);    // true
a.indexOf(res);     // -1
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
0420 - 069          // 203   : 0 --> base 8 0x --> hex
null + 0            // 0
1 + 2 + "3"         // '33'
1 + 2 + +"3"        // 6
0/0                 // NaN - according to google, its undefined
1/0                 // Infinity
-1/0                // -Infinity
!5 + !5             // 0
+!!NaN * "" - - [,] // 0 -> 0 * 0 - -0; // -> 0
```

some random number related stuff
```javascript
3 > 2 > 1   // false
true > 1    // false
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


## String

```javascript
(69).toString()             // '69'
(69).toString(2)            // '1000101'
69 + ''                     // '69'

'b' + 'a' + 'a' + 'a'       // 'baa'
'b' + 'a' + + 'a' + 'a'     // 'baNaNa'
'😎'.length                // 2

string.replace
string.replaceAll // dont fall for it
const uuid = 'e851e2fa-4f00-4609-9dd2-9b3794c59619'
console.log(string.replace('-', ''))    // e851e2fa4f00-4609-9dd2-9b3794c59619

"" - - "" // These two empty strings are both converted to 0.
0 - - 0; // -> 0

--"";   // SyntaxError, space is important
```

## Regex
Regex with the global flag is stateful. When the regex is global, if you call a method on the same regex object, 
it will start from the index past the end of the last match. When no more matches are found, the index is reset to 0 automatically.
```javascript
reg = /ab/g
str = "abc"
reg.test(str)   // true
reg.test(str)   // false

str.match(reg)  // ["ab"]
```
##### Conclusion
Don't do REGEX.
Match regex on base64 image data which is 15MB of characters. Just dont.

## Arrays
```javascript
[] + []                 // ''
[].toString();          // -> ""
new String() + [1,2,3]  // '1,2,3'
+[1].toString()         // 1
+[1,2].toString()       // NaN
[1] + [2]               // '12'
[1,2] + [3,4]           // '1,23,4'
[...[1,2], ...[3,4]]    // [1,2,3,4]
```

```javascript
[] + [] === [,] + [,];  // -> true
const arr = Array(15)   // [ <15 empty items> ]
arr.length              // 15
arr.join('wat')         // 'watwatwatwatwatwatwatwatwatwatwatwatwatwat'
arr.join('wat' + 1)     // 'wat1wat1wat1wat1wat1wat1wat1wat1wat1wat1wat1wat1wat1wat1'
arr.join('wat' - 1)     // 'NaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaN'
arr.join('wat' - 1) + ' Batman!' // 'NaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaNNaN Batman!'
```

```javascript
const test = [1,2,3];
test.length            // 3
delete test[1];
test.length            // 3
```
Deleting in arrays is likely not what you want.

```javascript
[1,2,3,4][1,3,2]         // 3
```
The index expression always uses the last expression, [1,3,2] --> [1,2,3,4][2]

## Objects
```javascript
[] + {} 
{} + []
{} + {}
[] + []

const asdf = { 6.0: '6Komma0', test: 'test' };
console.log(asdf[6]);
console.log(asdf['6']);
```

## JSON parsing
```javascript
JSON.parse("1")                       // 1
JSON.parse("-0")                      // -0
JSON.parse("10e5")                    // 1000000
JSON.parse("0x1")                     // SyntaxError
JSON.stringify(NaN)                   // null
JSON.stringify(Infinity)              // null
JSON.stringify(undefined)             // undefined
JSON.parse(JSON.stringify(undefined)) // error
JSON.stringify({foo: undefined})      // {}
JSON.stringify([])                    // ‘[]’
JSON.stringify([undefined])           // ‘[null]’
JSON.parse("9007199254740995")        // 9007199254740996
JSON.stringify({options: {"": "", "1": "1"}}) // reordered keys '{"options":{"1":"1","":""}}'
```

## Date
JavaScript's Date constructor has some peculiar behavior when dealing with numeric strings.

```javascript
// A numeric string between 32 and 49 is assumed to be in the 2000s:
console.log( new Date( "49" ) );
// Result: Date Fri Jan 01 2049 00:00:00 GMT-0500 (Eastern Standard Time)

// A numeric string between 50 and 99 is assumed to be in the 1900s:
console.log( new Date( "99" ) );
// Result: Date Fri Jan 01 1999 00:00:00 GMT-0500 (Eastern Standard Time)

// ...But 100 and up start from year zero:
console.log( new Date( "100" ) );
// Result: Date Fri Jan 01 0100 00:00:00 GMT-0456 (Eastern Standard Time)
```

## Weird Stuff
Key & value are swapped between for..of and map
```javascript
const map = new Map();
for(const [k,v] of map){
    console.log(k,v);
}

map.forEach((v,k) => {
    console.log(k,v);
});
```
Undefined is a defined value
```javascript
let test = undefined;
test == undefined   // true
asdf == undefined   // ReferenceError
```
```javascript
```

And then the fun stuff. Obfuscation via array / object additions.
```javascript
'W'+
'h'+
(![]+[])[+!![]]+
(!![]+[])[+[]]+
(+{}+{})[+!![]+[+[]]]+
(!![]+[])[+[]]+
'h'+
([]+{})[+!![]+[+!![]]]+
(+{}+{})[+!![]+[+[]]]+
([]+{})[+!![]+[+[]]]+
(![]+[])[+!![]]+
'v'+
(![]+[])[+!![]]+
(![]+[])[!+[]+!![]+!![]]+
(![]+{})[+!![]+[+[]]]+
(!![]+[])[+!![]]+
(![]+[]+[][[]])[+!![]+[+[]]]+
'p'+
(!![]+[])[+[]]
```
