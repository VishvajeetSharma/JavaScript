# JavaScript Arrays --- 3 Years Experience Interview Guide

> A practical interview-preparation guide covering JavaScript array
> methods, tricky output questions, ES6+ patterns, performance,
> immutability, and real-world scenarios.

------------------------------------------------------------------------

# 1. JavaScript Array Methods --- Complete Reference

## 1.1 `Array.isArray()`

Checks whether a value is an array.

``` js
Array.isArray([]);
Array.isArray([1, 2, 3]);
Array.isArray({});
Array.isArray("hello");
```

Output:

``` js
true
true
false
false
```

### Interview Question

**Q: Why use `Array.isArray(value)` instead of
`typeof value === "array"`?**

**Answer:** `typeof []` returns `"object"`. JavaScript does not have
`"array"` as a `typeof` result. `Array.isArray()` is the reliable way to
check for arrays.

------------------------------------------------------------------------

# 2. Creating Arrays

## 2.1 Array Literal

``` js
const arr = [1, 2, 3];
```

## 2.2 `new Array()`

``` js
const arr = new Array(1, 2, 3);
```

## Important Trap

``` js
const a = new Array(3);
console.log(a);
console.log(a.length);
```

Output:

``` js
[empty × 3]
3
```

It creates an array with length `3`, not `[3]`.

``` js
const a = Array(3);
const b = [3];

console.log(a); // [empty × 3]
console.log(b); // [3]
```

------------------------------------------------------------------------

# 3. `at()`

Returns the element at an index.

``` js
const arr = [10, 20, 30, 40];

console.log(arr.at(2));  // 30
console.log(arr.at(-1)); // 40
console.log(arr.at(-2)); // 30
```

### Why use `at()`?

It makes negative indexing easy.

``` js
arr[arr.length - 1]
```

can become:

``` js
arr.at(-1)
```

### Interview Question

**Q: Does `at()` mutate the array?**

**Answer:** No.

------------------------------------------------------------------------

# 4. `push()`

Adds one or more elements to the end.

``` js
const arr = [1, 2];

const result = arr.push(3, 4);

console.log(arr);    // [1, 2, 3, 4]
console.log(result); // 4
```

### Important

`push()` mutates the original array and returns the new length.

------------------------------------------------------------------------

# 5. `pop()`

Removes the last element.

``` js
const arr = [1, 2, 3];

const result = arr.pop();

console.log(result); // 3
console.log(arr);    // [1, 2]
```

Mutates the array.

------------------------------------------------------------------------

# 6. `unshift()`

Adds elements to the beginning.

``` js
const arr = [2, 3];

const result = arr.unshift(0, 1);

console.log(arr);    // [0, 1, 2, 3]
console.log(result); // 4
```

Mutates the array.

------------------------------------------------------------------------

# 7. `shift()`

Removes the first element.

``` js
const arr = [1, 2, 3];

const result = arr.shift();

console.log(result); // 1
console.log(arr);    // [2, 3]
```

Mutates the array.

------------------------------------------------------------------------

# 8. `concat()`

Combines arrays without mutating the original.

``` js
const a = [1, 2];
const b = [3, 4];

const result = a.concat(b);

console.log(result); // [1, 2, 3, 4]
console.log(a);      // [1, 2]
```

Can also accept values:

``` js
[1, 2].concat(3, [4, 5]);
// [1, 2, 3, 4, 5]
```

------------------------------------------------------------------------

# 9. `slice()`

Returns a shallow copy of a portion of an array.

``` js
const arr = [10, 20, 30, 40, 50];

console.log(arr.slice(1, 4));
// [20, 30, 40]
```

The end index is excluded.

### Negative indexes

``` js
arr.slice(-2);
// [40, 50]
```

### Does it mutate?

No.

------------------------------------------------------------------------

# 10. `splice()`

Adds/removes/replaces elements.

``` js
const arr = [1, 2, 3, 4];

const removed = arr.splice(1, 2);

console.log(removed); // [2, 3]
console.log(arr);     // [1, 4]
```

### Add without removing

``` js
const arr = [1, 4];

arr.splice(1, 0, 2, 3);

console.log(arr);
// [1, 2, 3, 4]
```

### Replace

``` js
const arr = [1, 2, 3];

arr.splice(1, 1, 20);

console.log(arr);
// [1, 20, 3]
```

### Important

`splice()` mutates the original array.

------------------------------------------------------------------------

# 11. `toSpliced()`

Modern non-mutating version of `splice()`.

``` js
const arr = [1, 2, 3, 4];

const result = arr.toSpliced(1, 2);

console.log(result); // [1, 4]
console.log(arr);    // [1, 2, 3, 4]
```

### Interview Question

**Q: Difference between `splice()` and `toSpliced()`?**

**Answer:**

-   `splice()` mutates the original array.
-   `toSpliced()` returns a new array.
-   `toSpliced()` is useful when immutable state updates are required.

------------------------------------------------------------------------

# 12. `with()`

Returns a new array with one element replaced.

``` js
const arr = [10, 20, 30];

const result = arr.with(1, 200);

console.log(result); // [10, 200, 30]
console.log(arr);    // [10, 20, 30]
```

Negative indexes work:

``` js
arr.with(-1, 999);
```

------------------------------------------------------------------------

# 13. `fill()`

Fills elements with a value.

``` js
const arr = [1, 2, 3, 4];

arr.fill(0);

console.log(arr);
// [0, 0, 0, 0]
```

Range:

``` js
const arr = [1, 2, 3, 4, 5];

arr.fill(0, 1, 4);

console.log(arr);
// [1, 0, 0, 0, 5]
```

Mutates the original.

### Important Object Trap

``` js
const arr = new Array(3).fill({ count: 0 });

arr[0].count++;

console.log(arr);
```

Output:

``` js
[
  { count: 1 },
  { count: 1 },
  { count: 1 }
]
```

All positions reference the same object.

------------------------------------------------------------------------

# 14. `copyWithin()`

Copies part of an array to another position.

``` js
const arr = [1, 2, 3, 4, 5];

arr.copyWithin(0, 3);

console.log(arr);
// [4, 5, 3, 4, 5]
```

It mutates the array.

------------------------------------------------------------------------

# 15. `indexOf()`

Returns the first matching index.

``` js
const arr = [10, 20, 30, 20];

console.log(arr.indexOf(20)); // 1
console.log(arr.indexOf(99)); // -1
```

Uses strict equality (`===`).

### `NaN` trap

``` js
console.log([NaN].indexOf(NaN));
// -1
```

------------------------------------------------------------------------

# 16. `lastIndexOf()`

Returns the last matching index.

``` js
const arr = [10, 20, 30, 20];

console.log(arr.lastIndexOf(20));
// 3
```

------------------------------------------------------------------------

# 17. `includes()`

Checks whether an array contains a value.

``` js
[1, 2, 3].includes(2);
// true
```

Unlike `indexOf()`, `includes()` can correctly find `NaN`.

``` js
[NaN].includes(NaN);
// true
```

### Interview Question

**Q: `includes()` vs `indexOf()`?**

**Answer:**

-   `includes()` returns a boolean.
-   `indexOf()` returns an index or `-1`.
-   `includes()` uses SameValueZero comparison, so `NaN` matches `NaN`.

------------------------------------------------------------------------

# 18. `find()`

Returns the first element matching a condition.

``` js
const users = [
  { id: 1, name: "A" },
  { id: 2, name: "B" }
];

const user = users.find(user => user.id === 2);

console.log(user);
// { id: 2, name: "B" }
```

If nothing matches:

``` js
undefined
```

------------------------------------------------------------------------

# 19. `findIndex()`

Returns the index of the first matching element.

``` js
const arr = [10, 20, 30];

console.log(arr.findIndex(x => x > 15));
// 1
```

If nothing matches:

``` js
-1
```

------------------------------------------------------------------------

# 20. `findLast()`

Returns the last matching element.

``` js
const arr = [10, 20, 30, 40];

console.log(arr.findLast(x => x > 15));
// 40
```

------------------------------------------------------------------------

# 21. `findLastIndex()`

Returns the index of the last matching element.

``` js
const arr = [10, 20, 30, 40];

console.log(arr.findLastIndex(x => x > 15));
// 3
```

------------------------------------------------------------------------

# 22. `filter()`

Returns a new array containing matching elements.

``` js
const arr = [1, 2, 3, 4, 5];

const result = arr.filter(x => x % 2 === 0);

console.log(result);
// [2, 4]
```

Does not mutate the original.

### Important

`filter()` always returns an array.

------------------------------------------------------------------------

# 23. `map()`

Transforms every element and returns a new array.

``` js
const arr = [1, 2, 3];

const result = arr.map(x => x * 2);

console.log(result);
// [2, 4, 6]
```

### `map()` vs `forEach()`

``` js
const result = arr.map(x => x * 2);
```

returns a new array.

``` js
const result = arr.forEach(x => x * 2);
```

returns:

``` js
undefined
```

------------------------------------------------------------------------

# 24. `forEach()`

Executes a function for each element.

``` js
const arr = [1, 2, 3];

arr.forEach((value, index) => {
  console.log(index, value);
});
```

Return value:

``` js
undefined
```

### Important

`forEach()` does not support normal `break` or `continue`.

------------------------------------------------------------------------

# 25. `some()`

Checks whether at least one element passes the condition.

``` js
const arr = [1, 2, 3, 4];

console.log(arr.some(x => x > 3));
// true
```

Stops early when it finds a match.

------------------------------------------------------------------------

# 26. `every()`

Checks whether every element passes the condition.

``` js
const arr = [2, 4, 6];

console.log(arr.every(x => x % 2 === 0));
// true
```

Stops early when a condition fails.

------------------------------------------------------------------------

# 27. `reduce()`

Reduces an array into a single value.

``` js
const arr = [1, 2, 3, 4];

const sum = arr.reduce((acc, value) => {
  return acc + value;
}, 0);

console.log(sum);
// 10
```

### Object result

``` js
const users = ["A", "B", "A"];

const counts = users.reduce((acc, name) => {
  acc[name] = (acc[name] || 0) + 1;
  return acc;
}, {});

console.log(counts);
// { A: 2, B: 1 }
```

------------------------------------------------------------------------

# 28. `reduceRight()`

Same concept as `reduce()`, but processes from right to left.

``` js
const arr = ["a", "b", "c"];

const result = arr.reduceRight(
  (acc, value) => acc + value,
  ""
);

console.log(result);
// "cba"
```

------------------------------------------------------------------------

# 29. `flat()`

Flattens nested arrays.

``` js
const arr = [1, [2, [3, 4]]];

console.log(arr.flat());
// [1, 2, [3, 4]]

console.log(arr.flat(2));
// [1, 2, 3, 4]
```

Default depth is `1`.

### Completely flatten

``` js
arr.flat(Infinity);
```

------------------------------------------------------------------------

# 30. `flatMap()`

Maps and then flattens one level.

``` js
const arr = [1, 2, 3];

const result = arr.flatMap(x => [x, x * 2]);

console.log(result);
// [1, 2, 2, 4, 3, 6]
```

### Difference

``` js
arr.map(...).flat()
```

is conceptually similar to:

``` js
arr.flatMap(...)
```

But `flatMap()` performs a single-level flattening as part of the
operation.

------------------------------------------------------------------------

# 31. `sort()`

Sorts an array in place.

### Number sorting trap

``` js
const arr = [10, 2, 30, 4];

arr.sort();

console.log(arr);
```

Output:

``` js
[10, 2, 30, 4]
```

Why?

Because default sorting converts values to strings and compares them
lexicographically.

### Correct numeric sort

``` js
arr.sort((a, b) => a - b);
```

Descending:

``` js
arr.sort((a, b) => b - a);
```

### Object sorting

``` js
users.sort((a, b) => a.age - b.age);
```

### Important

`sort()` mutates the array.

------------------------------------------------------------------------

# 32. `toSorted()`

Non-mutating version of `sort()`.

``` js
const arr = [10, 2, 30];

const result = arr.toSorted((a, b) => a - b);

console.log(result);
// [2, 10, 30]

console.log(arr);
// [10, 2, 30]
```

------------------------------------------------------------------------

# 33. `reverse()`

Reverses an array in place.

``` js
const arr = [1, 2, 3];

arr.reverse();

console.log(arr);
// [3, 2, 1]
```

------------------------------------------------------------------------

# 34. `toReversed()`

Non-mutating version of `reverse()`.

``` js
const arr = [1, 2, 3];

const result = arr.toReversed();

console.log(result);
// [3, 2, 1]

console.log(arr);
// [1, 2, 3]
```

------------------------------------------------------------------------

# 35. `join()`

Converts array elements into a string.

``` js
const arr = ["JavaScript", "React", "Node"];

console.log(arr.join(" - "));
// JavaScript - React - Node
```

Default separator:

``` js
arr.join();
// JavaScript,React,Node
```

------------------------------------------------------------------------

# 36. `toString()`

Converts the array to a string.

``` js
[1, 2, 3].toString();
// "1,2,3"
```

Usually equivalent to:

``` js
[1, 2, 3].join(",");
```

------------------------------------------------------------------------

# 37. `entries()`

Returns an iterator containing `[index, value]`.

``` js
const arr = ["a", "b"];

for (const [index, value] of arr.entries()) {
  console.log(index, value);
}
```

------------------------------------------------------------------------

# 38. `keys()`

Returns an iterator of indexes.

``` js
const arr = ["a", "b", "c"];

for (const key of arr.keys()) {
  console.log(key);
}
```

Output:

``` text
0
1
2
```

------------------------------------------------------------------------

# 39. `values()`

Returns an iterator of values.

``` js
for (const value of [10, 20, 30].values()) {
  console.log(value);
}
```

------------------------------------------------------------------------

# 40. `[Symbol.iterator]()`

Arrays are iterable.

``` js
const arr = [1, 2, 3];

for (const value of arr) {
  console.log(value);
}
```

The default iterator is equivalent to `values()`.

------------------------------------------------------------------------

# 41. `Array.from()`

Creates an array from an iterable or array-like value.

``` js
Array.from("hello");
// ["h", "e", "l", "l", "o"]
```

NodeList example:

``` js
const elements = document.querySelectorAll("div");

const arr = Array.from(elements);
```

### Mapping during conversion

``` js
Array.from([1, 2, 3], x => x * 2);
// [2, 4, 6]
```

------------------------------------------------------------------------

# 42. `Array.of()`

Creates an array from arguments.

``` js
Array.of(1, 2, 3);
// [1, 2, 3]
```

Important difference:

``` js
Array(3);
// [empty × 3]

Array.of(3);
// [3]
```

------------------------------------------------------------------------

# 43. `Array.fromAsync()`

Creates a promise for an array from an async iterable or array-like
source.

``` js
const result = await Array.fromAsync(
  async function* () {
    yield 1;
    yield 2;
    yield 3;
  }()
);

console.log(result);
// [1, 2, 3]
```

Useful with asynchronous iterables.

------------------------------------------------------------------------

# 44. `group()` and `groupToMap()`

Modern JavaScript environments may provide grouping helpers.

Conceptually:

``` js
const users = [
  { name: "A", role: "admin" },
  { name: "B", role: "user" },
  { name: "C", role: "admin" }
];

const grouped = Object.groupBy(users, user => user.role);
```

Result:

``` js
{
  admin: [
    { name: "A", role: "admin" },
    { name: "C", role: "admin" }
  ],
  user: [
    { name: "B", role: "user" }
  ]
}
```

For `Map` keys:

``` js
Map.groupBy(items, item => item.category);
```

Check your target runtime/browser support before using these in
production.

------------------------------------------------------------------------

# 45. Mutating vs Non-Mutating Array Methods

## Mutating

These change the original array:

``` text
push
pop
shift
unshift
splice
sort
reverse
fill
copyWithin
```

Modern mutation alternatives include:

``` text
toSpliced
toSorted
toReversed
with
```

which return new arrays.

## Non-Mutating

Common non-mutating methods:

``` text
at
concat
slice
includes
indexOf
lastIndexOf
find
findIndex
findLast
findLastIndex
filter
map
forEach
some
every
reduce
reduceRight
flat
flatMap
join
```

------------------------------------------------------------------------

# 46. Important Callback Parameters

Many array methods provide:

``` js
array.method((value, index, array) => {});
```

Example:

``` js
[10, 20, 30].map((value, index, array) => {
  console.log(value, index, array);
});
```

The callback generally receives:

1.  `value`
2.  `index`
3.  `array`

------------------------------------------------------------------------

# 47. `map()` Callback Trap

``` js
const result = ["1", "2", "3"].map(parseInt);

console.log(result);
```

Output:

``` js
[1, NaN, NaN]
```

Why?

`map()` passes:

``` js
(value, index)
```

So this becomes:

``` js
parseInt("1", 0);
parseInt("2", 1);
parseInt("3", 2);
```

Correct:

``` js
["1", "2", "3"].map(Number);
// [1, 2, 3]
```

------------------------------------------------------------------------

# 48. `filter(Boolean)`

Useful for removing falsy values.

``` js
const arr = [
  0,
  1,
  false,
  true,
  "",
  "hello",
  null,
  undefined,
  NaN
];

console.log(arr.filter(Boolean));
```

Result:

``` js
[1, true, "hello"]
```

Be careful: this also removes valid values such as `0` and `false`.

------------------------------------------------------------------------

# 49. `find()` vs `filter()`

``` js
const users = [
  { id: 1 },
  { id: 2 },
  { id: 2 }
];
```

Find one:

``` js
users.find(user => user.id === 2);
```

Returns the first matching object.

Filter all:

``` js
users.filter(user => user.id === 2);
```

Returns all matching objects.

------------------------------------------------------------------------

# 50. `some()` vs `every()`

``` js
const arr = [2, 4, 6];

arr.some(x => x > 5);
// true

arr.every(x => x % 2 === 0);
// true
```

Meaning:

-   `some()` = at least one
-   `every()` = all

------------------------------------------------------------------------

# 51. Empty Array Behavior

Important interview trap:

``` js
[].some(x => true);
// false

[].every(x => false);
// true

[].filter(x => true);
// []

[].map(x => x * 2);
// []

[].find(x => true);
// undefined
```

`every()` returning `true` for an empty array is called vacuous truth.

------------------------------------------------------------------------

# 52. Sparse Arrays

Consider:

``` js
const arr = new Array(3);

console.log(arr.length);
// 3
```

But it has no actual elements.

Compare:

``` js
const a = new Array(3);
const b = [undefined, undefined, undefined];

console.log(a.length === b.length);
// true
```

But they are structurally different because `a` has holes.

------------------------------------------------------------------------

# 53. Sparse Array and `map()`

``` js
const arr = new Array(3);

const result = arr.map(() => 1);

console.log(result);
// [empty × 3]
```

The callback is not called for missing elements.

------------------------------------------------------------------------

# 54. `Array.prototype.map()` Does Not Deep Clone

``` js
const users = [
  { name: "A" },
  { name: "B" }
];

const copy = users.map(user => user);

copy[0].name = "Changed";

console.log(users[0].name);
// Changed
```

Why?

Because the objects are still referenced.

------------------------------------------------------------------------

# 55. Shallow Copy

These create shallow copies:

``` js
const a = [...original];
const b = original.slice();
const c = Array.from(original);
const d = original.concat();
```

Nested objects are still shared.

------------------------------------------------------------------------

# 56. Array Reference Equality

``` js
const a = [];
const b = [];

console.log(a === b);
```

Output:

``` js
false
```

Each array is a different object/reference.

``` js
const a = [];
const b = a;

console.log(a === b);
// true
```

Both variables reference the same array.

------------------------------------------------------------------------

# 57. `[] == []` and `[] === []`

``` js
[] == [];
// false

[] === [];
// false
```

Because they are different object references.

------------------------------------------------------------------------

# 58. Array to Primitive Conversion

``` js
console.log([] + []);
```

Output:

``` text
""
```

``` js
console.log([1, 2] + [3, 4]);
```

Output:

``` text
"1,23,4"
```

Arrays are converted to strings during this `+` operation.

------------------------------------------------------------------------

# 59. `[] + {}`

``` js
console.log([] + {});
```

Typically:

``` text
"[object Object]"
```

This is a coercion question and should be explained through
`ToPrimitive`/string conversion rather than memorized.

------------------------------------------------------------------------

# 60. Interview Output Questions

## Q1

``` js
const a = [1, 2, 3];
const b = a;

b.push(4);

console.log(a);
```

### Answer

``` js
[1, 2, 3, 4]
```

Because `a` and `b` reference the same array.

------------------------------------------------------------------------

## Q2

``` js
const a = [1, 2, 3];
const b = [...a];

b.push(4);

console.log(a);
console.log(b);
```

### Answer

``` js
[1, 2, 3]
[1, 2, 3, 4]
```

Spread creates a shallow copy.

------------------------------------------------------------------------

## Q3

``` js
const arr = [1, 2, 3];

console.log(arr.push(4));
console.log(arr);
```

### Answer

``` js
4
[1, 2, 3, 4]
```

`push()` returns the new length.

------------------------------------------------------------------------

## Q4

``` js
const arr = [1, 2, 3];

console.log(arr.pop());
console.log(arr);
```

### Answer

``` js
3
[1, 2]
```

------------------------------------------------------------------------

## Q5

``` js
const arr = [1, 2, 3];

console.log(arr.shift());
console.log(arr);
```

### Answer

``` js
1
[2, 3]
```

------------------------------------------------------------------------

## Q6

``` js
const arr = [1, 2, 3];

console.log(arr.unshift(0));
console.log(arr);
```

### Answer

``` js
4
[0, 1, 2, 3]
```

------------------------------------------------------------------------

## Q7

``` js
const arr = [1, 2, 3, 4];

console.log(arr.slice(1, 3));
console.log(arr);
```

### Answer

``` js
[2, 3]
[1, 2, 3, 4]
```

------------------------------------------------------------------------

## Q8

``` js
const arr = [1, 2, 3, 4];

console.log(arr.splice(1, 2));
console.log(arr);
```

### Answer

``` js
[2, 3]
[1, 4]
```

------------------------------------------------------------------------

## Q9

``` js
const arr = [10, 2, 5, 1];

arr.sort();

console.log(arr);
```

### Answer

``` js
[1, 10, 2, 5]
```

Default sort is lexicographic.

------------------------------------------------------------------------

## Q10

``` js
const arr = [10, 2, 5, 1];

console.log(arr.toSorted((a, b) => a - b));
console.log(arr);
```

### Answer

``` js
[1, 2, 5, 10]
[10, 2, 5, 1]
```

------------------------------------------------------------------------

## Q11

``` js
const arr = [1, 2, 3];

const result = arr.map(x => {
  x * 2;
});

console.log(result);
```

### Answer

``` js
[undefined, undefined, undefined]
```

There is no `return`.

------------------------------------------------------------------------

## Q12

``` js
const arr = [1, 2, 3];

const result = arr.forEach(x => x * 2);

console.log(result);
```

### Answer

``` js
undefined
```

`forEach()` returns `undefined`.

------------------------------------------------------------------------

## Q13

``` js
const arr = [1, 2, 3];

console.log(arr.filter(x => x > 10));
```

### Answer

``` js
[]
```

------------------------------------------------------------------------

## Q14

``` js
const arr = [1, 2, 3];

console.log(arr.find(x => x > 10));
```

### Answer

``` js
undefined
```

------------------------------------------------------------------------

## Q15

``` js
const arr = [1, 2, 3];

console.log(arr.findIndex(x => x > 10));
```

### Answer

``` js
-1
```

------------------------------------------------------------------------

## Q16

``` js
console.log([1, 2, 3].includes(2));
console.log([1, 2, 3].includes(5));
```

### Answer

``` js
true
false
```

------------------------------------------------------------------------

## Q17

``` js
console.log([NaN].includes(NaN));
console.log([NaN].indexOf(NaN));
```

### Answer

``` js
true
-1
```

------------------------------------------------------------------------

## Q18

``` js
const arr = [1, 2, 3, 4];

console.log(arr.some(x => x % 2 === 0));
```

### Answer

``` js
true
```

------------------------------------------------------------------------

## Q19

``` js
const arr = [1, 2, 3, 4];

console.log(arr.every(x => x % 2 === 0));
```

### Answer

``` js
false
```

------------------------------------------------------------------------

## Q20

``` js
console.log([].some(() => true));
console.log([].every(() => false));
```

### Answer

``` js
false
true
```

------------------------------------------------------------------------

# 61. `reduce()` Interview Questions

## Q21

``` js
console.log([1, 2, 3, 4].reduce((a, b) => a + b, 0));
```

### Answer

``` js
10
```

------------------------------------------------------------------------

## Q22

``` js
console.log([1, 2, 3].reduce((a, b) => a * b, 1));
```

### Answer

``` js
6
```

------------------------------------------------------------------------

## Q23

What happens here?

``` js
[].reduce((a, b) => a + b);
```

### Answer

It throws:

``` text
TypeError
```

because there is no initial value and no first element available to use
as the accumulator.

------------------------------------------------------------------------

## Q24

``` js
console.log([1, 2, 3].reduce((a, b) => a + b));
```

### Answer

``` js
6
```

Without an initial value, `1` becomes the initial accumulator and
iteration starts with `2`.

------------------------------------------------------------------------

# 62. Flattening Interview Questions

## Q25

``` js
const arr = [1, [2, [3, [4]]]];

console.log(arr.flat());
```

### Answer

``` js
[1, 2, [3, [4]]]
```

Default depth is `1`.

------------------------------------------------------------------------

## Q26

``` js
console.log([1, [2, [3, [4]]]].flat(Infinity));
```

### Answer

``` js
[1, 2, 3, 4]
```

------------------------------------------------------------------------

## Q27

``` js
console.log([1, 2, 3].flatMap(x => [x, x * 2]));
```

### Answer

``` js
[1, 2, 2, 4, 3, 6]
```

------------------------------------------------------------------------

# 63. `sort()` Interview Questions

## Q28

``` js
console.log([100, 2, 30, 4].sort());
```

### Answer

``` js
[100, 2, 30, 4]
```

String comparison produces this ordering.

------------------------------------------------------------------------

## Q29

Sort numbers descending.

### Answer

``` js
arr.sort((a, b) => b - a);
```

------------------------------------------------------------------------

## Q30

Sort users by age.

``` js
const users = [
  { name: "A", age: 30 },
  { name: "B", age: 20 },
  { name: "C", age: 25 }
];
```

### Answer

``` js
users.sort((a, b) => a.age - b.age);
```

------------------------------------------------------------------------

# 64. Object Array Interview Questions

## Q31 --- Find User by ID

``` js
const users = [
  { id: 1, name: "A" },
  { id: 2, name: "B" }
];
```

### Answer

``` js
const user = users.find(user => user.id === 2);
```

------------------------------------------------------------------------

## Q32 --- Get All User Names

### Answer

``` js
const names = users.map(user => user.name);
```

------------------------------------------------------------------------

## Q33 --- Get Active Users

``` js
const users = [
  { name: "A", active: true },
  { name: "B", active: false },
  { name: "C", active: true }
];
```

### Answer

``` js
const activeUsers = users.filter(user => user.active);
```

------------------------------------------------------------------------

## Q34 --- Check Whether Any User Is Admin

``` js
const users = [
  { role: "user" },
  { role: "admin" }
];
```

### Answer

``` js
const hasAdmin = users.some(user => user.role === "admin");
```

------------------------------------------------------------------------

## Q35 --- Check Whether Every User Is Verified

``` js
const allVerified = users.every(user => user.verified);
```

------------------------------------------------------------------------

# 65. Remove Duplicates

## Q36 --- Primitive Values

``` js
const arr = [1, 2, 2, 3, 3, 4];

const unique = [...new Set(arr)];

console.log(unique);
// [1, 2, 3, 4]
```

------------------------------------------------------------------------

# 66. Remove Duplicate Objects by ID

``` js
const users = [
  { id: 1, name: "A" },
  { id: 2, name: "B" },
  { id: 1, name: "A" }
];

const unique = [
  ...new Map(users.map(user => [user.id, user])).values()
];
```

This keeps the last object for duplicate IDs.

------------------------------------------------------------------------

# 67. Find Duplicate Values

``` js
const arr = [1, 2, 2, 3, 4, 4];

const duplicates = [
  ...new Set(
    arr.filter((value, index) => arr.indexOf(value) !== index)
  )
];

console.log(duplicates);
// [2, 4]
```

For large arrays, a frequency map/set approach is usually more
efficient.

------------------------------------------------------------------------

# 68. Frequency Counter

``` js
const arr = ["a", "b", "a", "c", "b", "a"];

const frequency = arr.reduce((acc, value) => {
  acc[value] = (acc[value] || 0) + 1;
  return acc;
}, {});

console.log(frequency);
```

Output:

``` js
{
  a: 3,
  b: 2,
  c: 1
}
```

------------------------------------------------------------------------

# 69. Find Maximum

``` js
const arr = [10, 50, 20, 80];

const max = Math.max(...arr);

console.log(max);
// 80
```

For very large arrays, avoid blindly spreading huge arrays into a
function call; use a loop/reduce when appropriate.

------------------------------------------------------------------------

# 70. Find Minimum

``` js
const min = Math.min(...arr);
```

------------------------------------------------------------------------

# 71. Sum Array

``` js
const sum = arr.reduce((sum, value) => sum + value, 0);
```

------------------------------------------------------------------------

# 72. Average

``` js
const average =
  arr.reduce((sum, value) => sum + value, 0) / arr.length;
```

Handle empty arrays explicitly in production.

------------------------------------------------------------------------

# 73. Chunk an Array

``` js
function chunk(arr, size) {
  const result = [];

  for (let i = 0; i < arr.length; i += size) {
    result.push(arr.slice(i, i + size));
  }

  return result;
}

console.log(chunk([1, 2, 3, 4, 5], 2));
```

Output:

``` js
[
  [1, 2],
  [3, 4],
  [5]
]
```

------------------------------------------------------------------------

# 74. Flatten Without `flat()`

``` js
const arr = [1, [2, [3, 4]]];

const result = arr.reduce((acc, value) => {
  return acc.concat(
    Array.isArray(value)
      ? value.reduce((a, v) => a.concat(v), [])
      : value
  );
}, []);
```

For interviews, recursion is often a cleaner way to demonstrate
arbitrary-depth flattening.

------------------------------------------------------------------------

# 75. Deep Flatten Using Recursion

``` js
function flatten(arr) {
  const result = [];

  for (const value of arr) {
    if (Array.isArray(value)) {
      result.push(...flatten(value));
    } else {
      result.push(value);
    }
  }

  return result;
}
```

------------------------------------------------------------------------

# 76. Intersection of Arrays

``` js
const a = [1, 2, 3, 4];
const b = [3, 4, 5, 6];

const intersection = a.filter(value => b.includes(value));

console.log(intersection);
// [3, 4]
```

For large arrays, convert one side to a `Set`:

``` js
const setB = new Set(b);

const intersection = a.filter(value => setB.has(value));
```

------------------------------------------------------------------------

# 77. Union of Arrays

``` js
const union = [...new Set([...a, ...b])];
```

------------------------------------------------------------------------

# 78. Difference of Arrays

``` js
const difference = a.filter(value => !b.includes(value));
```

Using a `Set` for larger arrays:

``` js
const setB = new Set(b);

const difference = a.filter(value => !setB.has(value));
```

------------------------------------------------------------------------

# 79. Move Array Element

``` js
function move(arr, from, to) {
  const result = [...arr];
  const [item] = result.splice(from, 1);
  result.splice(to, 0, item);
  return result;
}
```

This preserves the original array.

------------------------------------------------------------------------

# 80. Rotate Array

``` js
function rotate(arr, k) {
  const n = arr.length;

  if (n === 0) return [];

  k = k % n;

  return [
    ...arr.slice(-k),
    ...arr.slice(0, -k)
  ];
}
```

------------------------------------------------------------------------

# 81. Array Method Complexity

Typical complexity:

  Method                      Typical Time Mutates?
  -------------- ------------------------- ----------
  `push()`                  O(1) amortized Yes
  `pop()`                             O(1) Yes
  `shift()`                           O(n) Yes
  `unshift()`                         O(n) Yes
  `slice()`                           O(k) No
  `splice()`                          O(n) Yes
  `map()`                             O(n) No
  `filter()`                          O(n) No
  `find()`                 O(n) worst case No
  `some()`                 O(n) worst case No
  `every()`                O(n) worst case No
  `reduce()`                          O(n) No\*
  `sort()`            Typically O(n log n) Yes
  `includes()`                        O(n) No
  `indexOf()`                         O(n) No
  `flat()`         Depends on output/depth No
  `reverse()`                         O(n) Yes

`reduce()` itself does not mutate the array unless your callback mutates
something.

------------------------------------------------------------------------

# 82. Important Interview Concept --- Mutation

### Example

``` js
const users = [{ name: "A" }];

const result = users.map(user => {
  user.name = "B";
  return user;
});
```

Both `users` and `result` contain the modified object.

Why?

Because `map()` creates a new array but does not deep clone objects.

------------------------------------------------------------------------

# 83. Immutable Update Patterns

Instead of:

``` js
arr.push(newItem);
```

Use:

``` js
const next = [...arr, newItem];
```

Instead of:

``` js
arr.splice(index, 1);
```

Use:

``` js
const next = arr.toSpliced(index, 1);
```

or:

``` js
const next = [
  ...arr.slice(0, index),
  ...arr.slice(index + 1)
];
```

Instead of:

``` js
arr.sort(compare);
```

Use:

``` js
const next = arr.toSorted(compare);
```

------------------------------------------------------------------------

# 84. React Interview Question

**Q: Why should you avoid directly mutating arrays in React state?**

### Answer

React state updates are easier to reason about when treated immutably.
Direct mutation can cause reference-equality checks to miss changes and
can lead to stale or unexpected UI behavior.

Bad:

``` js
users.push(newUser);
setUsers(users);
```

Better:

``` js
setUsers(prev => [...prev, newUser]);
```

For removal:

``` js
setUsers(prev =>
  prev.filter(user => user.id !== id)
);
```

For update:

``` js
setUsers(prev =>
  prev.map(user =>
    user.id === id
      ? { ...user, name: "New Name" }
      : user
  )
);
```

------------------------------------------------------------------------

# 85. Array Destructuring

``` js
const [first, second, third] = [10, 20, 30];

console.log(first);
// 10
```

Skip values:

``` js
const [first, , third] = [10, 20, 30];
```

Rest:

``` js
const [first, ...rest] = [1, 2, 3, 4];

console.log(first);
// 1

console.log(rest);
// [2, 3, 4]
```

------------------------------------------------------------------------

# 86. Swap Array Elements

``` js
[arr[0], arr[1]] = [arr[1], arr[0]];
```

------------------------------------------------------------------------

# 87. Spread vs `concat()`

``` js
const result = [...a, ...b];
```

and:

``` js
const result = a.concat(b);
```

Both can combine arrays.

Spread is especially convenient when constructing a new array with other
values:

``` js
[0, ...a, 99]
```

------------------------------------------------------------------------

# 88. Array-Like vs Array

An array-like object may have:

``` js
{
  0: "a",
  1: "b",
  length: 2
}
```

but it does not necessarily have array methods.

Convert it:

``` js
Array.from(arrayLike);
```

------------------------------------------------------------------------

# 89. `arguments` and Arrays

Traditional `arguments` is array-like, not a true array.

``` js
function test() {
  console.log(Array.isArray(arguments));
}
```

Output:

``` js
false
```

Convert:

``` js
const args = Array.from(arguments);
```

------------------------------------------------------------------------

# 90. `Array.from()` vs Spread

For an iterable:

``` js
[..."hello"];
```

and:

``` js
Array.from("hello");
```

both produce:

``` js
["h", "e", "l", "l", "o"]
```

`Array.from()` also supports a mapping function:

``` js
Array.from("123", Number);
```

------------------------------------------------------------------------

# 91. `for...of` vs `for...in`

For arrays:

``` js
for (const value of arr) {
  console.log(value);
}
```

iterates values.

``` js
for (const index in arr) {
  console.log(index);
}
```

iterates enumerable property keys.

For arrays, prefer `for...of` when you want values.

------------------------------------------------------------------------

# 92. `forEach()` vs `for...of`

`forEach()`:

``` js
arr.forEach(item => {
  console.log(item);
});
```

`for...of`:

``` js
for (const item of arr) {
  console.log(item);
}
```

`for...of` supports:

``` js
break;
continue;
```

and can be used naturally with `await` inside an async function.

------------------------------------------------------------------------

# 93. Async `forEach()` Trap

Bad assumption:

``` js
await users.forEach(async user => {
  await saveUser(user);
});

console.log("done");
```

`forEach()` does not await the returned promises.

For sequential processing:

``` js
for (const user of users) {
  await saveUser(user);
}
```

For parallel processing:

``` js
await Promise.all(
  users.map(user => saveUser(user))
);
```

This is a very common 3-year-experience interview question.

------------------------------------------------------------------------

# 94. `map()` with Async

This:

``` js
const result = users.map(async user => {
  return await getUser(user.id);
});
```

returns:

``` js
Promise[]
```

Use:

``` js
const result = await Promise.all(
  users.map(user => getUser(user.id))
);
```

------------------------------------------------------------------------

# 95. `Promise.all()` + Arrays

``` js
const promises = [
  fetch("/api/users"),
  fetch("/api/products"),
  fetch("/api/orders")
];

const results = await Promise.all(promises);
```

Results maintain the input order, although individual operations may
finish at different times.

------------------------------------------------------------------------

# 96. `reduce()` for Sequential Async Work

Possible:

``` js
await items.reduce(
  (promise, item) =>
    promise.then(() => process(item)),
  Promise.resolve()
);
```

But a normal `for...of` loop is generally easier to read for sequential
async work.

------------------------------------------------------------------------

# 97. Array Method Chaining

``` js
const result = users
  .filter(user => user.active)
  .map(user => user.name)
  .sort();
```

Read it as:

1.  Keep active users.
2.  Extract names.
3.  Sort names.

------------------------------------------------------------------------

# 98. Interview Question --- `filter().map()` vs `map().filter()`

These are not generally equivalent.

``` js
arr
  .filter(x => x > 0)
  .map(x => x * 2);
```

does less mapping work if many values are filtered out.

Where appropriate, operation ordering can affect performance.

------------------------------------------------------------------------

# 99. Interview Question --- Can `map()` Skip Elements?

For a sparse array, yes.

``` js
const arr = [1, , 3];

const result = arr.map(x => x * 2);

console.log(result);
```

The hole remains a hole.

------------------------------------------------------------------------

# 100. Interview Question --- Does `filter()` Preserve Order?

Yes.

The retained elements appear in their original relative order.

------------------------------------------------------------------------

# 101. Interview Question --- Does `sort()` preserve original array?

No.

``` js
const arr = [3, 1, 2];

const result = arr.sort();

console.log(result === arr);
// true
```

`sort()` returns the same array reference after mutating it.

------------------------------------------------------------------------

# 102. Interview Question --- What does `reverse()` return?

It returns the same mutated array.

``` js
const arr = [1, 2, 3];

const result = arr.reverse();

console.log(result === arr);
// true
```

------------------------------------------------------------------------

# 103. Interview Question --- What does `splice()` return?

It returns an array containing the removed elements.

``` js
const arr = [1, 2, 3];

const removed = arr.splice(1, 1);

console.log(removed);
// [2]
```

------------------------------------------------------------------------

# 104. Interview Question --- What does `slice()` return?

It returns a new shallow-copied array.

``` js
const arr = [1, 2, 3];

const result = arr.slice();

console.log(result === arr);
// false
```

------------------------------------------------------------------------

# 105. Interview Question --- `slice()` vs `splice()`

  `slice()`                `splice()`
  ------------------------ --------------------------------
  Does not mutate          Mutates
  Returns copied portion   Returns removed items
  Good for copying         Good for insert/delete/replace
  End index excluded       Delete count is explicit

------------------------------------------------------------------------

# 106. Interview Question --- `map()` vs `forEach()`

  `map()`                     `forEach()`
  --------------------------- ----------------------------------------
  Returns new array           Returns undefined
  Used for transformation     Used for side effects
  Chainable                   Not useful for transformation chaining
  Does not mutate by itself   Does not mutate by itself

------------------------------------------------------------------------

# 107. Interview Question --- `find()` vs `findIndex()`

``` js
find()
```

returns the element.

``` js
findIndex()
```

returns its index.

Example:

``` js
const arr = [10, 20, 30];

arr.find(x => x > 15);
// 20

arr.findIndex(x => x > 15);
// 1
```

------------------------------------------------------------------------

# 108. Interview Question --- `find()` vs `filter()`

`find()` stops after the first match.

`filter()` checks the array and returns all matches.

For one matching element, `find()` expresses the intention more directly
and can short-circuit.

------------------------------------------------------------------------

# 109. Interview Question --- `some()` vs `find()`

``` js
some()
```

answers:

> Does at least one element satisfy this condition?

``` js
find()
```

answers:

> Which is the first element satisfying this condition?

------------------------------------------------------------------------

# 110. Interview Question --- `reduce()` vs `map()`

`map()` transforms each element into a new array.

`reduce()` can transform an entire array into any accumulator result:

``` js
number
string
object
array
Map
Set
etc.
```

------------------------------------------------------------------------

# 111. Interview Question --- Is `reduce()` always better?

No.

For simple transformations:

``` js
arr.map(...)
```

is usually clearer.

Use `reduce()` when the problem naturally requires accumulation.

------------------------------------------------------------------------

# 112. Interview Question --- What happens if `push()` receives an array?

``` js
const arr = [1, 2];

arr.push([3, 4]);

console.log(arr);
```

Answer:

``` js
[1, 2, [3, 4]]
```

To append individual values:

``` js
arr.push(...[3, 4]);
```

------------------------------------------------------------------------

# 113. Interview Question --- `concat()` Does Not Deep Flatten

``` js
[1].concat([2, [3, 4]]);
```

Result:

``` js
[1, 2, [3, 4]]
```

Only one level of array arguments is concatenated.

------------------------------------------------------------------------

# 114. Interview Question --- Shallow Copy Trap

``` js
const a = [
  { value: 10 }
];

const b = [...a];

b[0].value = 100;

console.log(a[0].value);
```

Answer:

``` js
100
```

The outer array was copied, but the nested object reference was not.

------------------------------------------------------------------------

# 115. Deep Clone

For supported data, one modern option is:

``` js
const clone = structuredClone(original);
```

JSON cloning:

``` js
JSON.parse(JSON.stringify(original));
```

has many limitations and should not be treated as a general deep-cloning
solution.

------------------------------------------------------------------------

# 116. Interview Question --- `new Array(5).map(...)`

``` js
const result = new Array(5).map(() => 1);

console.log(result);
```

Answer:

``` js
[empty × 5]
```

The array contains holes, so `map()` has no elements to visit.

Correct:

``` js
Array.from({ length: 5 }, () => 1);
```

Result:

``` js
[1, 1, 1, 1, 1]
```

------------------------------------------------------------------------

# 117. Interview Question --- Create Array of N Numbers

``` js
const arr = Array.from(
  { length: 5 },
  (_, index) => index
);

console.log(arr);
```

Output:

``` js
[0, 1, 2, 3, 4]
```

------------------------------------------------------------------------

# 118. Interview Question --- Create 1 to N

``` js
const n = 5;

const arr = Array.from(
  { length: n },
  (_, index) => index + 1
);

console.log(arr);
```

Output:

``` js
[1, 2, 3, 4, 5]
```

------------------------------------------------------------------------

# 119. Interview Question --- Second Largest Number

``` js
function secondLargest(arr) {
  const unique = [...new Set(arr)].sort((a, b) => b - a);
  return unique[1];
}
```

For interview discussion, mention that sorting costs approximately O(n
log n). A one-pass solution can achieve O(n).

------------------------------------------------------------------------

# 120. One-Pass Largest and Second Largest

``` js
function secondLargest(arr) {
  let largest = -Infinity;
  let second = -Infinity;

  for (const value of arr) {
    if (value > largest) {
      second = largest;
      largest = value;
    } else if (value > second && value !== largest) {
      second = value;
    }
  }

  return second;
}
```

Time: O(n)

Extra space: O(1)

Discuss what should happen if fewer than two distinct values exist.

------------------------------------------------------------------------

# 121. Interview Question --- Move Zeros to End

``` js
function moveZeros(arr) {
  const nonZero = arr.filter(x => x !== 0);
  const zeros = arr.filter(x => x === 0);

  return [...nonZero, ...zeros];
}
```

This preserves relative order.

------------------------------------------------------------------------

# 122. Interview Question --- Count Even Numbers

``` js
const count = arr.filter(x => x % 2 === 0).length;
```

Alternative:

``` js
const count = arr.reduce(
  (count, value) => count + (value % 2 === 0 ? 1 : 0),
  0
);
```

------------------------------------------------------------------------

# 123. Interview Question --- First Duplicate

``` js
function firstDuplicate(arr) {
  const seen = new Set();

  for (const value of arr) {
    if (seen.has(value)) {
      return value;
    }

    seen.add(value);
  }

  return undefined;
}
```

Time: O(n) average.

Space: O(n).

------------------------------------------------------------------------

# 124. Interview Question --- Check if Two Arrays Have Same Values

For primitive values and order-sensitive comparison:

``` js
function areEqual(a, b) {
  return (
    a.length === b.length &&
    a.every((value, index) => value === b[index])
  );
}
```

------------------------------------------------------------------------

# 125. Interview Question --- Same Values Ignoring Order

If duplicates matter:

``` js
function sameValues(a, b) {
  if (a.length !== b.length) return false;

  const count = new Map();

  for (const value of a) {
    count.set(value, (count.get(value) || 0) + 1);
  }

  for (const value of b) {
    const current = count.get(value);

    if (!current) return false;

    count.set(value, current - 1);
  }

  return true;
}
```

This handles duplicate frequencies.

------------------------------------------------------------------------

# 126. Interview Question --- Remove Falsy Values

``` js
const result = arr.filter(Boolean);
```

But be ready to explain that this removes:

``` text
false
0
""
null
undefined
NaN
```

------------------------------------------------------------------------

# 127. Interview Question --- Convert Array of Strings to Numbers

``` js
const result = ["10", "20", "30"].map(Number);
```

Do not blindly use:

``` js
arr.map(parseInt)
```

because of the callback index.

------------------------------------------------------------------------

# 128. Interview Question --- Sum Nested Object Values

``` js
const orders = [
  { amount: 100 },
  { amount: 200 },
  { amount: 50 }
];

const total = orders.reduce(
  (sum, order) => sum + order.amount,
  0
);
```

------------------------------------------------------------------------

# 129. Interview Question --- Group by Property

Using a reducer:

``` js
const grouped = users.reduce((acc, user) => {
  const key = user.role;

  if (!acc[key]) {
    acc[key] = [];
  }

  acc[key].push(user);

  return acc;
}, {});
```

------------------------------------------------------------------------

# 130. Interview Question --- Pagination

Given:

``` js
const page = 2;
const limit = 10;
```

Calculate:

``` js
const start = (page - 1) * limit;

const result = data.slice(start, start + limit);
```

For real APIs, server-side pagination is generally preferable for large
datasets.

------------------------------------------------------------------------

# 131. Interview Question --- Search by Multiple Conditions

``` js
const result = users.filter(user =>
  user.active &&
  user.age >= 18 &&
  user.role === "user"
);
```

------------------------------------------------------------------------

# 132. Interview Question --- Update One Object

``` js
const updated = users.map(user =>
  user.id === targetId
    ? { ...user, active: false }
    : user
);
```

This creates a new array and a new object only for the matching item.

------------------------------------------------------------------------

# 133. Interview Question --- Delete One Object

``` js
const updated = users.filter(
  user => user.id !== targetId
);
```

------------------------------------------------------------------------

# 134. Interview Question --- Insert at Index Without Mutation

``` js
const updated = [
  ...arr.slice(0, index),
  newItem,
  ...arr.slice(index)
];
```

------------------------------------------------------------------------

# 135. Interview Question --- Replace at Index Without Mutation

Modern:

``` js
const updated = arr.with(index, newValue);
```

Compatible alternative:

``` js
const updated = arr.map((value, i) =>
  i === index ? newValue : value
);
```

------------------------------------------------------------------------

# 136. Interview Question --- Remove by Index Without Mutation

Modern:

``` js
const updated = arr.toSpliced(index, 1);
```

Compatible alternative:

``` js
const updated = [
  ...arr.slice(0, index),
  ...arr.slice(index + 1)
];
```

------------------------------------------------------------------------

# 137. Interview Question --- Why `arr.length = 0` Is Different?

``` js
arr.length = 0;
```

This mutates the existing array object.

If other variables reference it:

``` js
const a = [1, 2, 3];
const b = a;

a.length = 0;

console.log(b);
// []
```

By contrast:

``` js
a = [];
```

when reassignment is possible, changes the variable's reference rather
than clearing the original object.

------------------------------------------------------------------------

# 138. Interview Question --- Is `const arr = []` Immutable?

No.

`const` prevents reassignment of the variable:

``` js
const arr = [];

arr.push(1); // allowed
```

But:

``` js
arr = [1];
```

throws because the binding cannot be reassigned.

------------------------------------------------------------------------

# 139. Interview Question --- Array as Object

``` js
const arr = [10, 20];

arr.name = "test";

console.log(arr.name);
// test
```

Arrays are objects and can have properties, although adding arbitrary
properties to arrays is generally not a good design.

------------------------------------------------------------------------

# 140. Interview Question --- `length` Can Mutate Array

``` js
const arr = [1, 2, 3, 4];

arr.length = 2;

console.log(arr);
// [1, 2]
```

Increase:

``` js
arr.length = 5;
```

creates holes.

------------------------------------------------------------------------

# 141. Interview Question --- Delete Array Element

``` js
const arr = [1, 2, 3];

delete arr[1];

console.log(arr);
```

The array becomes sparse:

``` js
[1, empty, 3]
```

Its length remains `3`.

Usually prefer:

``` js
arr.splice(1, 1);
```

if you want to remove an element and shift later indexes.

------------------------------------------------------------------------

# 142. Interview Question --- Why Avoid `delete arr[index]`?

Because it creates a hole instead of shifting elements and can introduce
sparse-array behavior.

Use `splice()` for mutation or `toSpliced()` for an immutable operation.

------------------------------------------------------------------------

# 143. Interview Question --- `Object.keys(array)`

``` js
const arr = ["a", "b"];

console.log(Object.keys(arr));
```

Output:

``` js
["0", "1"]
```

The keys are strings.

------------------------------------------------------------------------

# 144. Interview Question --- Array Equality

Why?

``` js
[1, 2] === [1, 2]
```

is:

``` js
false
```

Because arrays are objects and object equality checks
identity/reference, not contents.

------------------------------------------------------------------------

# 145. Interview Question --- How to Compare Nested Arrays?

For simple JSON-compatible data:

``` js
JSON.stringify(a) === JSON.stringify(b)
```

can work when ordering and serialization semantics are appropriate.

But it is not a universal deep-equality solution.

For production applications, use a tested deep-equality implementation
when necessary.

------------------------------------------------------------------------

# 146. Interview Question --- `Set` vs Array for Membership

Array:

``` js
arr.includes(value);
```

Usually O(n).

Set:

``` js
set.has(value);
```

Typically O(1) average lookup.

If you perform many membership checks on a large collection, a `Set` can
be more appropriate.

------------------------------------------------------------------------

# 147. Interview Question --- `Map` vs Array Search

Instead of repeatedly:

``` js
users.find(user => user.id === id);
```

you can index users:

``` js
const userMap = new Map(
  users.map(user => [user.id, user])
);

userMap.get(id);
```

This is useful when many lookups are required.

------------------------------------------------------------------------

# 148. Interview Question --- Why `includes()` Is Better Than `indexOf() !== -1`

Instead of:

``` js
arr.indexOf(value) !== -1
```

you can write:

``` js
arr.includes(value)
```

which communicates intent more clearly.

It also correctly detects `NaN`.

------------------------------------------------------------------------

# 149. Interview Question --- Does `map()` Mutate?

The `map()` method itself does not mutate the original array.

But the callback can mutate objects or the array:

``` js
arr.map(item => {
  item.active = true;
  return item;
});
```

So distinguish:

> method behavior

from:

> what the callback does.

------------------------------------------------------------------------

# 150. Interview Question --- Can Array Methods Be Chained?

Yes.

``` js
const result = users
  .filter(user => user.active)
  .map(user => user.name)
  .filter(Boolean)
  .sort();
```

Each method returns a value that can be passed to the next method.

------------------------------------------------------------------------

# 151. Interview Question --- Which Methods Short-Circuit?

Important short-circuiting methods include:

``` text
some()
every()
find()
findIndex()
findLast()
findLastIndex()
```

They can stop before processing every element.

`filter()`, `map()`, `forEach()`, and `reduce()` normally process the
relevant elements through the complete traversal.

------------------------------------------------------------------------

# 152. Interview Question --- Which Array Methods Mutate?

Memorize:

``` text
push
pop
shift
unshift
splice
sort
reverse
fill
copyWithin
```

Modern non-mutating alternatives:

``` text
toSpliced
toSorted
toReversed
with
```

------------------------------------------------------------------------

# 153. Interview Question --- What Is a Shallow Copy?

A shallow copy creates a new outer array but retains references to
nested objects.

``` js
const a = [{ x: 1 }];
const b = [...a];

console.log(a === b);
// false

console.log(a[0] === b[0]);
// true
```

------------------------------------------------------------------------

# 154. Interview Question --- Array Destructuring Default

``` js
const [a = 10] = [undefined];

console.log(a);
// 10
```

But:

``` js
const [a = 10] = [null];

console.log(a);
// null
```

Defaults apply to `undefined`, not `null`.

------------------------------------------------------------------------

# 155. Interview Question --- Rest Must Be Last

Valid:

``` js
const [first, ...rest] = arr;
```

Invalid:

``` js
const [...rest, last] = arr;
```

A rest element must be last in an array destructuring pattern.

------------------------------------------------------------------------

# 156. Interview Question --- Spread Is Shallow

``` js
const original = [[1], [2]];

const copy = [...original];

copy[0].push(99);

console.log(original);
// [[1, 99], [2]]
```

------------------------------------------------------------------------

# 157. Interview Question --- Array Constructor Trap

``` js
console.log(Array(3));
console.log(Array.of(3));
```

Answer:

``` js
[empty × 3]
[3]
```

This is one of the most common array constructor questions.

------------------------------------------------------------------------

# 158. Interview Question --- `Array.from({length: 3})`

``` js
console.log(Array.from({ length: 3 }));
```

Output:

``` js
[undefined, undefined, undefined]
```

Unlike `new Array(3)`, the resulting array has actual `undefined` values
that can be visited by array iteration methods.

------------------------------------------------------------------------

# 159. Interview Question --- `map()` vs `Array.from()`

This fails to invoke the callback for holes:

``` js
new Array(3).map(() => 1);
```

This works:

``` js
Array.from({ length: 3 }, () => 1);
```

------------------------------------------------------------------------

# 160. Interview Question --- Array-like Object

``` js
const obj = {
  0: "A",
  1: "B",
  length: 2
};

const arr = Array.from(obj);

console.log(arr);
// ["A", "B"]
```

------------------------------------------------------------------------

# 161. Practical Senior-Level Scenario --- Optimize Repeated Search

Bad for many searches:

``` js
for (const id of ids) {
  users.find(user => user.id === id);
}
```

Potentially O(n × m).

Better:

``` js
const userMap = new Map(
  users.map(user => [user.id, user])
);

for (const id of ids) {
  userMap.get(id);
}
```

Building the map costs O(n), and lookups are typically O(1) average.

------------------------------------------------------------------------

# 162. Practical Scenario --- Transform API Response

``` js
const cards = response.data
  .filter(item => item.status === "active")
  .map(item => ({
    id: item.id,
    title: item.name,
    price: item.price
  }));
```

This pattern is common in React/React Native applications.

------------------------------------------------------------------------

# 163. Practical Scenario --- Deduplicate API Results

``` js
const uniqueUsers = [
  ...new Map(
    users.map(user => [user.id, user])
  ).values()
];
```

Know whether you want the first or last duplicate to survive.

------------------------------------------------------------------------

# 164. Practical Scenario --- Group API Results

``` js
const grouped = users.reduce((acc, user) => {
  const role = user.role;

  (acc[role] ??= []).push(user);

  return acc;
}, {});
```

------------------------------------------------------------------------

# 165. Practical Scenario --- Build Dropdown Options

``` js
const options = users.map(user => ({
  label: user.name,
  value: user.id
}));
```

------------------------------------------------------------------------

# 166. Practical Scenario --- Search + Filter

``` js
const result = users.filter(user =>
  user.name
    .toLowerCase()
    .includes(search.toLowerCase())
);
```

For large datasets, consider server-side searching or indexing rather
than filtering a huge client-side array.

------------------------------------------------------------------------

# 167. Practical Scenario --- Stable Sorting

``` js
const sorted = users.toSorted(
  (a, b) => a.name.localeCompare(b.name)
);
```

`toSorted()` keeps the original array untouched.

------------------------------------------------------------------------

# 168. `localeCompare()` for Strings

Avoid:

``` js
a.name > b.name
```

for many human-language sorting requirements.

Prefer:

``` js
a.name.localeCompare(b.name)
```

Options:

``` js
a.name.localeCompare(
  b.name,
  undefined,
  { sensitivity: "base" }
);
```

------------------------------------------------------------------------

# 169. Interview Question --- Numeric `sort()` Comparator

What does this mean?

``` js
(a, b) => a - b
```

Answer:

-   negative → `a` comes before `b`
-   zero → equal ordering according to comparator
-   positive → `b` comes before `a`

------------------------------------------------------------------------

# 170. Interview Question --- Why `sort()` Comparator Can Be Dangerous

Bad:

``` js
arr.sort((a, b) => a > b);
```

This returns `true`/`false`, which are coerced to `1`/`0`, not a proper
three-way comparison.

Prefer:

``` js
arr.sort((a, b) => a - b);
```

for numbers.

------------------------------------------------------------------------

# 171. Interview Question --- `NaN` and Array Methods

``` js
const arr = [NaN];

console.log(arr.includes(NaN));
// true

console.log(arr.indexOf(NaN));
// -1
```

This difference is frequently tested.

------------------------------------------------------------------------

# 172. Interview Question --- `null` vs `undefined`

``` js
[null].includes(undefined);
// false

[undefined].includes(undefined);
// true
```

They are different values.

------------------------------------------------------------------------

# 173. Interview Question --- `filter()` Does Not Modify Length In Place

``` js
const arr = [1, 2, 3];

const result = arr.filter(x => x > 1);

console.log(arr);
// [1, 2, 3]

console.log(result);
// [2, 3]
```

------------------------------------------------------------------------

# 174. Interview Question --- Callback Index

``` js
const arr = ["a", "b", "c"];

arr.map((value, index) => {
  console.log(value, index);
});
```

Output:

``` text
a 0
b 1
c 2
```

------------------------------------------------------------------------

# 175. Interview Question --- Third Callback Argument

``` js
arr.map((value, index, array) => {
  console.log(array === arr);
});
```

For this ordinary call, it prints:

``` text
true
true
true
```

The third callback argument is the array being traversed.

------------------------------------------------------------------------

# 176. Interview Question --- Mutating During `map()`

Avoid code like:

``` js
arr.map((value, index) => {
  arr.push(value);
  return value;
});
```

Array iteration behavior around mutations is subtle and easy to make
incorrect. Do not depend on mutation during traversal unless you fully
understand the specification behavior.

------------------------------------------------------------------------

# 177. Interview Question --- `reduce()` Can Return an Array

``` js
const result = [1, 2, 3].reduce(
  (acc, value) => {
    acc.push(value * 2);
    return acc;
  },
  []
);

console.log(result);
// [2, 4, 6]
```

But `map()` is clearer for this exact transformation.

------------------------------------------------------------------------

# 178. Interview Question --- `reduce()` Can Build a Map

``` js
const userMap = users.reduce((map, user) => {
  map.set(user.id, user);
  return map;
}, new Map());
```

------------------------------------------------------------------------

# 179. Interview Question --- `reduce()` Can Build a Set

``` js
const unique = arr.reduce((set, value) => {
  set.add(value);
  return set;
}, new Set());
```

Though:

``` js
new Set(arr)
```

is simpler.

------------------------------------------------------------------------

# 180. Interview Question --- Which One Would You Choose?

### Requirement: transform every item

Use:

``` js
map()
```

### Requirement: keep matching items

Use:

``` js
filter()
```

### Requirement: first matching item

Use:

``` js
find()
```

### Requirement: determine whether any match

Use:

``` js
some()
```

### Requirement: determine whether all match

Use:

``` js
every()
```

### Requirement: accumulate into one result

Use:

``` js
reduce()
```

### Requirement: remove/insert in an array

Use:

``` js
splice()
```

or:

``` js
toSpliced()
```

for immutable code.

------------------------------------------------------------------------

# 181. Rapid-Fire Interview Questions

## Q181. Does `push()` return the pushed element?

**Answer:** No. It returns the new array length.

## Q182. Does `pop()` return the new length?

**Answer:** No. It returns the removed element.

## Q183. Does `shift()` mutate?

**Answer:** Yes.

## Q184. Does `unshift()` return an array?

**Answer:** No. It returns the new length.

## Q185. Does `slice()` mutate?

**Answer:** No.

## Q186. Does `splice()` mutate?

**Answer:** Yes.

## Q187. Does `sort()` mutate?

**Answer:** Yes.

## Q188. Does `toSorted()` mutate?

**Answer:** No.

## Q189. Does `reverse()` mutate?

**Answer:** Yes.

## Q190. Does `toReversed()` mutate?

**Answer:** No.

## Q191. Does `filter()` mutate?

**Answer:** No, unless the callback itself mutates something.

## Q192. Does `map()` mutate?

**Answer:** Not by itself.

## Q193. Does `forEach()` return an array?

**Answer:** No, it returns `undefined`.

## Q194. Does `find()` return an array?

**Answer:** No. It returns the first matching element or `undefined`.

## Q195. Does `findIndex()` return an element?

**Answer:** No. It returns an index or `-1`.

## Q196. Does `some()` return an array?

**Answer:** No. It returns a boolean.

## Q197. Does `every()` return an array?

**Answer:** No. It returns a boolean.

## Q198. What does `includes()` return?

**Answer:** Boolean.

## Q199. What does `indexOf()` return if not found?

**Answer:** `-1`.

## Q200. What does `find()` return if not found?

**Answer:** `undefined`.

------------------------------------------------------------------------

# 182. Final Interview Checklist

Before a 3-year JavaScript interview, you should be able to explain
without hesitation:

-   `map()` vs `forEach()`
-   `filter()` vs `find()`
-   `find()` vs `findIndex()`
-   `some()` vs `every()`
-   `reduce()` and accumulator
-   `slice()` vs `splice()`
-   `sort()` numeric comparator
-   `sort()` mutation
-   `toSorted()`
-   `reverse()` vs `toReversed()`
-   `splice()` vs `toSpliced()`
-   `includes()` vs `indexOf()`
-   `NaN` behavior
-   `flat()` and `flatMap()`
-   shallow copy vs deep copy
-   spread operator
-   array references
-   sparse arrays
-   `new Array(n)` trap
-   `Array.from()`
-   `Array.of()`
-   `Array.isArray()`
-   array destructuring
-   rest/spread
-   `Set` for deduplication
-   `Map` for indexing
-   async `map()`
-   `Promise.all()`
-   async `forEach()` trap
-   array time complexity
-   immutable array updates
-   React state array updates
-   grouping and frequency counting
-   pagination with `slice()`
-   chunking
-   intersection/union/difference
-   duplicate detection
-   sorting objects
-   API response transformation

------------------------------------------------------------------------

# 183. Most Important Methods to Memorize

``` text
push()
pop()
shift()
unshift()

slice()
splice()
toSpliced()

map()
filter()
forEach()

find()
findIndex()
findLast()
findLastIndex()

some()
every()

reduce()
reduceRight()

sort()
toSorted()

reverse()
toReversed()

includes()
indexOf()
lastIndexOf()

flat()
flatMap()

concat()
join()

fill()
copyWithin()

at()
with()

Array.isArray()
Array.from()
Array.fromAsync()
Array.of()

entries()
keys()
values()
```

------------------------------------------------------------------------

# 184. Final Rule for Interviews

Do not only memorize syntax.

For every array method, be ready to answer these five questions:

1.  **What does it return?**
2.  **Does it mutate the original array?**
3.  **What is its time complexity?**
4.  **Does it short-circuit?**
5.  **What happens on an empty/sparse array?**

If you can answer those five questions for the major methods, you are
prepared for most JavaScript array questions expected from a developer
with around 3 years of experience.
