# What the hell is this

This repo is a collection of resources, schemas and other stuff that I'm collecting while I'm learning FP for JS.

I'm not the author, I'm just putting third part text in order to have an schema that is useful for me.


## I've been looking at this resources:
- [Functional-Light JavaScript](https://github.com/getify/Functional-Light-JS) -- good first dive into FP.


# The basics
## Functions
In non-FP, functions could used as procedures (might have inputs o might have returned value).

In FP, functions are NOT procedures. All the functions must have inputs and outputs.

Functions always have to return values. Avoid premature return statement for logic purposes.

### Explicit over implicit
Consider create functions that not change anything outside the function. For example:
```js
var y;

function foo(x) {
	y = (2 * Math.pow( x, 2 )) + 3;
}

foo( 2 );

y;						// 11
```

In this case, `foo()` changes `y`value. That's called **side effect** in FP.

Consider this refactoring:

```js
function foo(x) {
	return (2 * Math.pow( x, 2 )) + 3;
}

var y = foo( 2 );

y;						// 11
```

In other cases, implicitness is not that obvious:

```js
function sum(list) {
	var total = 0;
	for (let i = 0; i < list.length; i++) {
		if (!list[i]) list[i] = 0;

		total = total + list[i];
	}

	return total;
}

var nums = [ 1, 3, 9, 27, , 84 ];

sum( nums );			// 124
```

In this case, `sum()` has side effects to `nums` Array. After execute it, `nums` mutate to `[ 1, 3, 9, 27, 0, 84 ]`.

A function that doesn't have side effects is called **pure function**.

### High-order functions
Functions that receives or returns one or more other function values.

Consider this examples:

```js
// Function that receives a function
function forEach(list, fn) {
	for (let i = 0; i < list.length; i++) {
		fn( list[i] );
	}
}

forEach( [1,2,3,4,5], function each(val){
	console.log( val );
} );
// 1 2 3 4 5
```

```js
// Function that returns a function
function foo() {
	var fn = function inner(msg){
		console.log( msg );
	};

	return fn;
}

var f = foo();

f( "Hello!" );			// Hello!
```

```js
// Not the only way to "return" a function
function foo() {
	var fn = function inner(msg){
		console.log( msg );
	};

	bar( fn );
}

function bar(func) {
	func( "Hello!" );
}

foo();					// Hello!
```
High-order functions treat other functions as values.

### Closures
Closure is when a function remembers and accesses variables from outside of its own scope, even that function is executed in a different scope.

```js
function foo(msg) {
	var fn = function inner(){
		console.log( msg );
	};

	return fn;
}

var helloFn = foo( "Hello!" );

helloFn();				// Hello!
```

If you have an operation that needs two inputs, one of which you know but the other will be specified later, you can use closure to remember the first input:

```js
function makeAdder(x) {
	return function sum(y){
		return x + y;
	};
}

// we already know `10` and `37` as first inputs, respectively
var addTo10 = makeAdder( 10 );
var addTo37 = makeAdder( 37 );

// later, we specify the second inputs
addTo10( 3 );			// 13
addTo10( 90 );			// 100

addTo37( 13 );			// 50
```
This technique of specifying inputs in successive function calls is very common in FP, 
