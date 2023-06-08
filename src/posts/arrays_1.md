---
title: Arrays for JavaScript beginners (Part 1)
slug: arrays_1
date: 2019-01-10
excerpt: Arrays are ordered collection of values. Each value is called an element and each element has a numeric position called index. Arrays in JavaScript are untyped meaning an array element may be of any data type...
image_link: https://cdn-images-1.medium.com/max/800/1*gNIR8wq441K74W_DGtlZOQ.jpeg
author: Bob Oyier
---

Arrays are ordered collection of values. Each value is called an element and each element has a numeric position called index.

Arrays in JavaScript are untyped meaning an array element may be of any data type.

They are also dynamic meaning they can grow and shrink as needed. There is no need to declare a fixed size of the array and also no need to reallocate memory when the array size changes.

Arrays may also be sparse meaning the elements don't need to have contiguous indexes. There may be gaps, and that's okay.

Arrays are just special objects. They are optimized to be accessed faster than objects meaning array elements can be accessed faster than object properties.

## Ways to create an array.

- Array literals
- The … spread operator
- The Array() constructor
- The Array.from() method
- The Array.of() method

### Array literals.

This is the simplest and most efficient way of creating an array. It is simply a comma-separated list of array elements within square brackets.

```js
const arr = ['a', 'b', 'c'];
```

If an array literal contains multiple commas in a row with no value between them, the array is sparse.

```js
const arr = [1, , , 2, 3, 4];
console.log(arr.indexOf(1)); // Element 1 is index 0.
console.log(arr.indexOf(2)); // Element 2 is index 3.
// There is no index 1 and 2 in this array.
```

### The `…` Spread operator

The three dots "spread" an existing array so that its elements become elements within a new array literal.

```js
const arr = ['a', 'b', 'c'];
const arr2 = [...arr, 1, 2, 3];
console.log(arr2); // => [ 'a', 'b', 'c', 1, 2, 3 ]
```

The spread operator can also create a shallow copy of an existing array.

```js
const arr = ['a', 'b', 'c'];
const arr2 = [...arr];
console.log(arr2 === arr); // => true
```

Modifying an array doesn't affect the original. It is non-destructive.

The spread operator works on any iterable object.

Check this out:

```js
const digits = [...'12345'];
console.log(digits); // => ['1', '2', '3', '4', '5'
```

### Array.of()

Since the Array() constructor cannot be invoked using a single numeric element (because it treats it as the length of an array), in comes Array.of()

```js
// Using an Array() constructor:
const numbers = new Array(5); // => [ , , , ,  ]

// Using Array.of() instead:
const numbers = Array.of(5); // => [5]
```

Array.of() can be used with any other data types.

```js
const arr = Array.of(true, false, 'hello', 3.14);
console.log(arr); // => [true, false, 'hello', 3.14]
```

### Array.from()

It expects any iterable as its first argument.

```js
const js = Array.from('JavaScript');
console.log(js); // => [ 'J', 'a', 'v', 'a', 'S', 'c', 'r', 'i', 'p', 't' ]
```

And it can also work as a spread operator.

```js
const arr = Array.from('abc');
const arr2 = Array.from(arr);
console.log(arr2); // => ['a', 'b', 'c']
```

### Array length in summary.

An array will never have an element whose index is greater than or equal to the array length. If you assign a value to an array element whose index i is greater than or equal to the array length, the array's length is set to i + 1

#### Using Array.length to manipulate arrays.

Consider the following code:

```js
const nums = [1, 2, 3, 4, 5, 6, 7];
console.log(nums.length); // => 7
```

The following code snippets explain how one can use Array.length to manipulate the structure of the array.

##### Truncating an array using array.length

```js
nums.length = 3;
console.log(nums); // => [1, 2, 3]
// Destructively truncates the array to the specified length.
```

##### Increasing the length of an array using array.length

```js
nums.length = 10;
console.log(nums); // => [1, 2, 3, , , , , , , ]
// Adds sparse areas to the tail end of the array.
```

##### Delete all elements of an array using array.length

```js
nums.length = 0;
console.log(nums); // => []
// Deletes everything.
```

### Adding and Deleting array elements.

Consider this empty array which we will build on to illustrate the following array methods:

```js
const arr = [];
```

#### Array.push()

This method adds one or more elements to the end of the array.

```js
arr.push('a', 'b', 'c');
console.log(arr); // => ['a', 'b', 'c']
```

#### Array.unshift()

This method adds one or more elements to the beginning of the array.

```js
arr.unshift('d', 'e');
console.log(arr); // => ['d', 'e', 'a', 'b', 'c']
```

This shifts the existing array elements to higher indexes.

#### Array.pop()

This method removes the last element from the array and returns that element.

```js
const popped = arr.pop();
console.log(arr); // => ['d', 'e', 'a', 'b']
console.log(popped); // => 'c'
```

#### Array.shift()

This method removes the first element from the array and returns that element.

```js
const shifted = arr.shift();
console.log(arr); // => ['e', 'a', 'b']
console.log(shifted); // => 'd'
```

**NOTE:**

You can delete an element in an array using the delete operator.

```js
const tens = [10, 20, 30, 40];
delete tens[2];
console.log(tens); // => [10, 20, , 40]
```

Using the delete operator does not affect the length of the array because it does not shift the elements of the array with higher indexes to fill the gap.

When used, that array becomes sparse.

## Conclusion

That was a brief introduction to JavaScript arrays. The next article will focus on Array higher-order functions (iteration methods) which give us powerful ways to manipulate arrays and unlock their versatility.

Hope you enjoyed this short article and feel free to leave any feedback.
