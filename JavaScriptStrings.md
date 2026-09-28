# JavaScript Strings --- 3 Years Experience Interview Q&A

## 1. Core String Methods

### `length`

``` js
"hello".length; // 5
```

Counts UTF-16 code units, not always visible characters.

### `at()`

``` js
"JavaScript".at(-1); // "t"
```

Supports negative indexes.

### `charAt()`

``` js
"hello".charAt(0);  // "h"
"hello".charAt(10); // ""
```

### `charCodeAt()`

``` js
"A".charCodeAt(0); // 65
```

Returns a UTF-16 code unit.

### `codePointAt()`

``` js
"😀".codePointAt(0); // 128512
```

Returns a Unicode code point.

### `concat()`

``` js
"Hello".concat(" ", "World");
// "Hello World"
```

### `includes()`

``` js
"JavaScript".includes("Script"); // true
```

Case-sensitive; returns boolean.

### `startsWith()`

``` js
"JavaScript".startsWith("Java"); // true
```

### `endsWith()`

``` js
"hello.js".endsWith(".js"); // true
```

### `indexOf()`

``` js
"hello world".indexOf("world"); // 6
"hello".indexOf("x");           // -1
```

### `lastIndexOf()`

``` js
"hello hello".lastIndexOf("hello"); // 6
```

### `search()`

``` js
"abc123".search(/\d+/); // 3
```

Accepts a regular expression and returns the match index.

### `match()`

``` js
"abc123 def456".match(/\d+/g);
// ["123", "456"]
```

### `matchAll()`

``` js
const matches = [..."a1 b2".matchAll(/([a-z])(\d)/g)];
```

Useful when all matches and capture groups are needed.

### `replace()`

``` js
"foo foo".replace("foo", "bar");
// "bar foo"
```

A plain string replaces the first occurrence.

### `replaceAll()`

``` js
"foo foo".replaceAll("foo", "bar");
// "bar bar"
```

### `split()`

``` js
"a,b,c".split(",");
// ["a", "b", "c"]
```

### `slice()`

``` js
"JavaScript".slice(0, 4); // "Java"
"JavaScript".slice(-6);  // "Script"
```

### `substring()`

``` js
"JavaScript".substring(0, 4); // "Java"
```

Negative values become `0`; if start \> end, arguments are swapped.

### `repeat()`

``` js
"ha".repeat(3); // "hahaha"
```

### `padStart()`

``` js
"5".padStart(3, "0"); // "005"
```

### `padEnd()`

``` js
"5".padEnd(3, "0"); // "500"
```

### `trim()`

``` js
"  hello  ".trim(); // "hello"
```

### `trimStart()`

``` js
"  hello  ".trimStart(); // "hello  "
```

### `trimEnd()`

``` js
"  hello  ".trimEnd(); // "  hello"
```

### `toUpperCase()` / `toLowerCase()`

``` js
"hello".toUpperCase(); // "HELLO"
"HELLO".toLowerCase(); // "hello"
```

### `toLocaleUpperCase()` / `toLocaleLowerCase()`

Use when locale-sensitive casing matters.

### `normalize()`

``` js
const a = "\u00E9";
const b = "e\u0301";

a === b; // false
a.normalize("NFC") === b.normalize("NFC"); // true
```

### `localeCompare()`

``` js
"a".localeCompare("b"); // negative
```

Only rely on negative/zero/positive, not exact numeric values.

### `isWellFormed()` / `toWellFormed()`

Modern Unicode helpers for detecting/fixing lone UTF-16 surrogates.

------------------------------------------------------------------------

# 2. String Creation

``` js
const a = "hello";
const b = 'hello';
const c = `hello`;
```

Template literals:

``` js
const name = "Vishvajeet";
const message = `Hello ${name}`;
```

Multiline:

``` js
const text = `
Hello
World
`;
```

------------------------------------------------------------------------

# 3. Primitive String vs String Object

``` js
const a = "hello";
const b = new String("hello");

typeof a; // "string"
typeof b; // "object"

a === b; // false
```

Prefer primitive strings.

------------------------------------------------------------------------

# 4. Strings Are Immutable

``` js
let str = "hello";

str[0] = "H";

console.log(str);
// "hello"
```

Correct:

``` js
str = "H" + str.slice(1);
```

Methods such as `trim()`, `replace()`, and `toUpperCase()` return new
strings.

------------------------------------------------------------------------

# 5. Character Access

``` js
const str = "hello";

str[0];          // "h"
str.at(-1);      // "o"
str.charAt(0);   // "h"
```

Important:

``` js
str[100];        // undefined
str.charAt(100); // ""
```

------------------------------------------------------------------------

# 6. Unicode and UTF-16

``` js
"😀".length;
// 2
```

JavaScript string length counts UTF-16 code units.

Code-point iteration:

``` js
[..."😀"].length;
// 1
```

For user-perceived characters, use `Intl.Segmenter` when appropriate:

``` js
const segmenter = new Intl.Segmenter(
  undefined,
  { granularity: "grapheme" }
);

const graphemes = [...segmenter.segment(text)];
```

------------------------------------------------------------------------

# 7. `charCodeAt()` vs `codePointAt()`

``` js
"A".charCodeAt(0);
// 65

"😀".codePointAt(0);
// 128512
```

`charCodeAt()` works with UTF-16 code units.

`codePointAt()` can represent the complete Unicode code point.

------------------------------------------------------------------------

# 8. `String.fromCharCode()` vs `String.fromCodePoint()`

``` js
String.fromCharCode(65);
// "A"

String.fromCodePoint(128512);
// "😀"
```

Use code-point APIs when Unicode code points are required.

------------------------------------------------------------------------

# 9. `slice()` vs `substring()`

``` js
"JavaScript".slice(-6);
// "Script"

"JavaScript".substring(-6);
// "JavaScript"
```

Also:

``` js
"hello".slice(4, 1);
// ""

"hello".substring(4, 1);
// "ell"
```

`substring()` converts negative values to `0` and swaps start/end.

------------------------------------------------------------------------

# 10. `replace()` vs `replaceAll()`

``` js
"foo foo".replace("foo", "bar");
// "bar foo"

"foo foo".replaceAll("foo", "bar");
// "bar bar"
```

Regex alternative:

``` js
"foo foo".replace(/foo/g, "bar");
```

------------------------------------------------------------------------

# 11. `includes()` vs `indexOf()`

``` js
str.includes("abc");
// true / false

str.indexOf("abc");
// index / -1
```

Use `includes()` when only presence is required.

------------------------------------------------------------------------

# 12. `search()` vs `indexOf()`

``` js
str.indexOf("123");
```

searches for a literal substring.

``` js
str.search(/\d+/);
```

accepts a regex.

------------------------------------------------------------------------

# 13. `match()` vs `matchAll()`

`match()`:

``` js
"abc123 def456".match(/\d+/g);
// ["123", "456"]
```

`matchAll()`:

``` js
[..."a1 b2".matchAll(/([a-z])(\d)/g)];
```

`matchAll()` is especially useful when every match's capture groups are
needed.

------------------------------------------------------------------------

# 14. `split()` Traps

``` js
"hello".split("");
// ["h", "e", "l", "l", "o"]

"hello".split();
// ["hello"]

"".split("");
// []
```

Whitespace:

``` js
"a   b".split(" ");
// ["a", "", "", "b"]

"a   b".trim().split(/\s+/);
// ["a", "b"]
```

------------------------------------------------------------------------

# 15. String Coercion

``` js
"5" + 2; // "52"
"5" - 2; // 3
"5" * 2; // 10
"5" / 2; // 2.5
```

`+` can concatenate strings; arithmetic operators generally coerce to
numbers.

------------------------------------------------------------------------

# 16. Important Coercion Outputs

``` js
1 + 2 + "3";
// "33"

"1" + 2 + 3;
// "123"

"hello" + true;
// "hellotrue"

"hello" + null;
// "hellonull"

"hello" + undefined;
// "helloundefined"
```

------------------------------------------------------------------------

# 17. `String()` vs `.toString()`

``` js
String(null);
// "null"

String(undefined);
// "undefined"
```

But:

``` js
null.toString();
// TypeError
```

`String(value)` is safer for general conversion.

------------------------------------------------------------------------

# 18. `Number()` vs `parseInt()`

``` js
Number("10.5");
// 10.5

parseInt("10.5", 10);
// 10

parseFloat("10.5px");
// 10.5

Number("10px");
// NaN
```

`parseInt()` parses an integer prefix; `Number()` attempts to convert
the whole value.

------------------------------------------------------------------------

# 19. `parseInt()` + `map()` Trap

``` js
["1", "2", "3"].map(parseInt);
```

Result:

``` js
[1, NaN, NaN]
```

Because `map()` passes `(value, index)`.

Correct:

``` js
["1", "2", "3"].map(Number);
// [1, 2, 3]
```

Or:

``` js
["1", "2", "3"].map(x => parseInt(x, 10));
```

------------------------------------------------------------------------

# 20. Regex Basics

Common flags:

``` text
g  global
i  case-insensitive
m  multiline
s  dotAll
u  Unicode
y  sticky
d  indices
```

Example:

``` js
/hello/gi
```

------------------------------------------------------------------------

# 21. Regex `test()`

``` js
/\d+/.test("abc123");
// true
```

Returns a boolean.

------------------------------------------------------------------------

# 22. Global Regex State Trap

``` js
const regex = /a/g;

console.log(regex.test("a"));
// true

console.log(regex.test("a"));
// false
```

Global regexes can maintain `lastIndex`.

------------------------------------------------------------------------

# 23. Capture Groups

``` js
const result = "name=John".match(/name=(\w+)/);

console.log(result[1]);
// "John"
```

Named groups:

``` js
const result =
  /(?<first>\w+)-(?<last>\w+)/.exec("John-Doe");

console.log(result.groups.first);
// "John"
```

------------------------------------------------------------------------

# 24. `String.raw()`

``` js
String.raw`C:\Users\Test`;
```

Useful when raw backslashes from a template literal are needed.

------------------------------------------------------------------------

# 25. Tagged Template Literals

``` js
function tag(strings, ...values) {
  console.log(strings);
  console.log(values);
}

const name = "John";

tag`Hello ${name}`;
```

A tag receives literal segments and interpolated values.

------------------------------------------------------------------------

# 26. Output Questions

## Q1

``` js
console.log("hello".length);
```

**Answer:** `5`

## Q2

``` js
console.log("hello"[10]);
```

**Answer:** `undefined`

## Q3

``` js
console.log("hello".charAt(10));
```

**Answer:** `""`

## Q4

``` js
console.log("hello".at(-1));
```

**Answer:** `"o"`

## Q5

``` js
console.log("JavaScript".slice(-6));
```

**Answer:** `"Script"`

## Q6

``` js
console.log("JavaScript".substring(-6));
```

**Answer:** `"JavaScript"`

## Q7

``` js
console.log("hello hello".indexOf("hello"));
```

**Answer:** `0`

## Q8

``` js
console.log("hello hello".lastIndexOf("hello"));
```

**Answer:** `6`

## Q9

``` js
console.log("Hello".includes("hello"));
```

**Answer:** `false`

## Q10

``` js
console.log("foo foo".replace("foo", "bar"));
```

**Answer:** `"bar foo"`

## Q11

``` js
console.log("foo foo".replaceAll("foo", "bar"));
```

**Answer:** `"bar bar"`

## Q12

``` js
console.log("a,b,c".split(","));
```

**Answer:** `["a", "b", "c"]`

## Q13

``` js
console.log("hello".repeat(3));
```

**Answer:** `"hellohellohello"`

## Q14

``` js
console.log("5".padStart(3, "0"));
```

**Answer:** `"005"`

## Q15

``` js
console.log("5".padEnd(3, "0"));
```

**Answer:** `"500"`

------------------------------------------------------------------------

# 27. More Output Questions

## Q16

``` js
console.log("5" + 2);
```

**Answer:** `"52"`

## Q17

``` js
console.log("5" - 2);
```

**Answer:** `3`

## Q18

``` js
console.log(1 + 2 + "3");
```

**Answer:** `"33"`

## Q19

``` js
console.log("1" + 2 + 3);
```

**Answer:** `"123"`

## Q20

``` js
console.log(Boolean(""));
```

**Answer:** `false`

## Q21

``` js
console.log(Boolean("false"));
```

**Answer:** `true`

Any non-empty string is truthy.

## Q22

``` js
console.log(Number(""));
```

**Answer:** `0`

## Q23

``` js
console.log(Number(" "));
```

**Answer:** `0`

## Q24

``` js
console.log(Number("10px"));
```

**Answer:** `NaN`

## Q25

``` js
console.log(parseInt("10px", 10));
```

**Answer:** `10`

------------------------------------------------------------------------

# 28. Primitive vs Object Questions

## Q26

``` js
const a = "hello";
const b = new String("hello");

console.log(a === b);
```

**Answer:** `false`

## Q27

``` js
console.log(typeof "hello");
console.log(typeof new String("hello"));
```

**Answer:**

``` text
string
object
```

## Q28

``` js
console.log(Boolean(new String("")));
```

**Answer:** `true`

Objects are truthy, even when their contained string is empty.

------------------------------------------------------------------------

# 29. Unicode Questions

## Q29

``` js
console.log("😀".length);
```

**Answer:** `2`

## Q30

``` js
console.log([..."😀"].length);
```

**Answer:** `1`

## Q31

Why?

**Answer:** `.length` counts UTF-16 code units. The spread operator uses
the string iterator and handles the emoji as one Unicode code point.

## Q32

Is `[...str].length` always the number of visible characters?

**Answer:** No. A grapheme cluster can contain multiple Unicode code
points. `Intl.Segmenter` with `granularity: "grapheme"` is more
appropriate for user-perceived character segmentation.

------------------------------------------------------------------------

# 30. Coding Questions

## Q33. Reverse a String

``` js
function reverseString(str) {
  return [...str].reverse().join("");
}
```

------------------------------------------------------------------------

## Q34. Check Palindrome

``` js
function isPalindrome(str) {
  const value = str.toLowerCase();

  return value === [...value].reverse().join("");
}
```

Define how spaces/punctuation/Unicode should be handled if the interview
requires normalization.

------------------------------------------------------------------------

## Q35. Count Vowels

``` js
function countVowels(str) {
  return [...str.toLowerCase()]
    .filter(char => "aeiou".includes(char))
    .length;
}
```

------------------------------------------------------------------------

## Q36. Remove Vowels

``` js
function removeVowels(str) {
  return str.replace(/[aeiou]/gi, "");
}
```

------------------------------------------------------------------------

## Q37. Count Character Frequency

``` js
function frequency(str) {
  return [...str].reduce((acc, char) => {
    acc[char] = (acc[char] || 0) + 1;
    return acc;
  }, {});
}
```

------------------------------------------------------------------------

## Q38. First Non-Repeating Character

``` js
function firstNonRepeating(str) {
  const count = new Map();

  for (const char of str) {
    count.set(char, (count.get(char) || 0) + 1);
  }

  for (const char of str) {
    if (count.get(char) === 1) {
      return char;
    }
  }

  return undefined;
}
```

Time: O(n) average.

Space: O(n).

------------------------------------------------------------------------

## Q39. First Repeating Character

``` js
function firstRepeating(str) {
  const seen = new Set();

  for (const char of str) {
    if (seen.has(char)) {
      return char;
    }

    seen.add(char);
  }

  return undefined;
}
```

------------------------------------------------------------------------

## Q40. Remove Duplicate Characters

``` js
function removeDuplicates(str) {
  return [...new Set(str)].join("");
}
```

------------------------------------------------------------------------

## Q41. Check Anagram

``` js
function isAnagram(a, b) {
  const normalize = str =>
    str.toLowerCase().replace(/\s/g, "");

  const x = [...normalize(a)].sort().join("");
  const y = [...normalize(b)].sort().join("");

  return x === y;
}
```

Sorting approach: approximately O(n log n).

------------------------------------------------------------------------

## Q42. Anagram in O(n)

``` js
function isAnagram(a, b) {
  if (a.length !== b.length) return false;

  const count = new Map();

  for (const char of a) {
    count.set(char, (count.get(char) || 0) + 1);
  }

  for (const char of b) {
    const current = count.get(char);

    if (!current) return false;

    count.set(char, current - 1);
  }

  return true;
}
```

------------------------------------------------------------------------

## Q43. Count Words

``` js
function countWords(str) {
  const value = str.trim();

  return value ? value.split(/\s+/).length : 0;
}
```

------------------------------------------------------------------------

## Q44. Reverse Words

``` js
function reverseWords(str) {
  return str.trim().split(/\s+/).reverse().join(" ");
}
```

------------------------------------------------------------------------

## Q45. Reverse Every Word

``` js
function reverseEachWord(str) {
  return str
    .split(" ")
    .map(word => [...word].reverse().join(""))
    .join(" ");
}
```

------------------------------------------------------------------------

## Q46. Longest Word

``` js
function longestWord(str) {
  return str
    .trim()
    .split(/\s+/)
    .reduce(
      (longest, word) =>
        word.length > longest.length ? word : longest,
      ""
    );
}
```

------------------------------------------------------------------------

## Q47. Shortest Word

``` js
function shortestWord(str) {
  const value = str.trim();

  if (!value) return "";

  return value.split(/\s+/).reduce(
    (shortest, word) =>
      word.length < shortest.length ? word : shortest
  );
}
```

------------------------------------------------------------------------

## Q48. Capitalize First Letter

``` js
function capitalize(str) {
  if (!str) return str;

  return str[0].toUpperCase() + str.slice(1);
}
```

------------------------------------------------------------------------

## Q49. Capitalize Every Word

``` js
function titleCase(str) {
  return str
    .trim()
    .split(/\s+/)
    .map(word =>
      word[0].toUpperCase() + word.slice(1).toLowerCase()
    )
    .join(" ");
}
```

------------------------------------------------------------------------

## Q50. Remove Extra Spaces

``` js
function normalizeSpaces(str) {
  return str.trim().replace(/\s+/g, " ");
}
```

------------------------------------------------------------------------

## Q51. Remove All Whitespace

``` js
function removeWhitespace(str) {
  return str.replace(/\s+/g, "");
}
```

------------------------------------------------------------------------

## Q52. Keep Only Digits

``` js
function onlyDigits(str) {
  return str.replace(/\D/g, "");
}
```

------------------------------------------------------------------------

## Q53. Check Digits Only

``` js
function isDigitsOnly(str) {
  return /^\d+$/.test(str);
}
```

------------------------------------------------------------------------

## Q54. Kebab Case

``` js
function kebabCase(str) {
  return str
    .trim()
    .toLowerCase()
    .replace(/\s+/g, "-");
}
```

------------------------------------------------------------------------

## Q55. Snake Case

``` js
function snakeCase(str) {
  return str
    .trim()
    .toLowerCase()
    .replace(/\s+/g, "_");
}
```

------------------------------------------------------------------------

## Q56. Camel Case to Kebab Case

``` js
function camelToKebab(str) {
  return str
    .replace(/([a-z])([A-Z])/g, "$1-$2")
    .toLowerCase();
}
```

------------------------------------------------------------------------

## Q57. String Rotation

``` js
function isRotation(a, b) {
  return (
    a.length === b.length &&
    (a + a).includes(b)
  );
}
```

Example:

``` js
isRotation("abcd", "cdab");
// true
```

------------------------------------------------------------------------

## Q58. Longest Common Prefix

``` js
function longestCommonPrefix(words) {
  if (!words.length) return "";

  let prefix = words[0];

  for (let i = 1; i < words.length; i++) {
    while (!words[i].startsWith(prefix)) {
      prefix = prefix.slice(0, -1);

      if (!prefix) return "";
    }
  }

  return prefix;
}
```

------------------------------------------------------------------------

## Q59. Longest Substring Without Repeating Characters

``` js
function longestUniqueSubstring(str) {
  const seen = new Map();

  let left = 0;
  let maxLength = 0;

  for (let right = 0; right < str.length; right++) {
    const char = str[right];

    if (seen.has(char)) {
      left = Math.max(left, seen.get(char) + 1);
    }

    seen.set(char, right);

    maxLength = Math.max(
      maxLength,
      right - left + 1
    );
  }

  return maxLength;
}
```

This version works on UTF-16 code units. A Unicode code-point or
grapheme-aware implementation requires different iteration.

------------------------------------------------------------------------

# 31. Practical Frontend/Backend String Questions

## Q60. Normalize Search Input

``` js
const query = input.trim().toLowerCase();
```

------------------------------------------------------------------------

## Q61. Search Users

``` js
const query = search.trim().toLowerCase();

const result = users.filter(user =>
  user.name.toLowerCase().includes(query)
);
```

------------------------------------------------------------------------

## Q62. Normalize Email

Common application normalization:

``` js
const email = input.trim().toLowerCase();
```

Do not assume every email system has identical case semantics.

------------------------------------------------------------------------

## Q63. Generate a Slug

``` js
function slugify(value) {
  return value
    .trim()
    .toLowerCase()
    .replace(/[^\w\s-]/g, "")
    .replace(/\s+/g, "-")
    .replace(/-+/g, "-");
}
```

For multilingual content, an ASCII-only regex may not be sufficient.

------------------------------------------------------------------------

## Q64. Mask a Phone Number

``` js
function maskPhone(phone) {
  const digits = phone.replace(/\D/g, "");

  if (digits.length <= 4) {
    return "*".repeat(digits.length);
  }

  return "*".repeat(digits.length - 4) + digits.slice(-4);
}
```

------------------------------------------------------------------------

## Q65. Mask an Email

``` js
function maskEmail(email) {
  const [name, domain] = email.split("@");

  if (!name || !domain) return email;
  if (name.length < 2) return email;

  return (
    name[0] +
    "*".repeat(name.length - 1) +
    "@" +
    domain
  );
}
```

------------------------------------------------------------------------

## Q66. Extract File Extension

``` js
function getExtension(filename) {
  const index = filename.lastIndexOf(".");

  return index === -1
    ? ""
    : filename.slice(index + 1);
}
```

------------------------------------------------------------------------

## Q67. Check Image Extension

``` js
function isImage(filename) {
  return /\.(jpg|jpeg|png|webp)$/i.test(filename);
}
```

------------------------------------------------------------------------

## Q68. Basic Email Validation

``` js
const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

emailRegex.test(email);
```

This is only a basic application-level check, not a complete
implementation of every valid email address rule.

------------------------------------------------------------------------

## Q69. Parse Query Parameters

Prefer:

``` js
const params = new URLSearchParams("?page=2&limit=10");

params.get("page");
// "2"
```

Instead of manually splitting the query string.

------------------------------------------------------------------------

## Q70. Convert Query Parameter to Number

``` js
const page = Number(params.get("page"));
```

Validate the result and bounds before using it.

------------------------------------------------------------------------

# 32. Security Interview Questions

## Q71. Is `innerHTML` safe for user input?

No.

Avoid:

``` js
element.innerHTML = userInput;
```

Prefer:

``` js
element.textContent = userInput;
```

or use a correctly configured sanitizer if HTML is genuinely required.

------------------------------------------------------------------------

## Q72. Can String Escaping Prevent SQL Injection?

No.

Do not construct SQL with:

``` js
const query =
  "SELECT * FROM users WHERE name = '" + name + "'";
```

Use parameterized queries or an ORM/query builder.

------------------------------------------------------------------------

## Q73. Is Removing HTML Tags With Regex Secure?

No.

This:

``` js
str.replace(/<[^>]*>/g, "");
```

is not a robust HTML parser or security boundary.

Use an HTML parser/sanitizer appropriate to the environment.

------------------------------------------------------------------------

# 33. Regex Interview Questions

## Q74. What does `g` mean?

Global matching.

## Q75. What does `i` mean?

Case-insensitive matching.

## Q76. What does `m` mean?

Multiline behavior for `^` and `$`.

## Q77. What does `u` mean?

Unicode-aware regex behavior.

## Q78. What does `s` mean?

DotAll: `.` can match line terminators.

## Q79. What does `y` mean?

Sticky matching from `lastIndex`.

## Q80. What does `d` mean?

Provides match indices in environments supporting it.

------------------------------------------------------------------------

# 34. Advanced Regex Questions

## Q81. What is a capture group?

``` js
/(\d+)/
```

The parentheses capture a part of the match.

## Q82. What is a named capture group?

``` js
/(?<id>\d+)/
```

Then:

``` js
result.groups.id
```

## Q83. Why can `/a/g.test("a")` behave differently on repeated calls?

Because global regexes maintain `lastIndex`.

## Q84. How do you reset it?

``` js
regex.lastIndex = 0;
```

------------------------------------------------------------------------

# 35. Performance Questions

## Q85. Is every string method O(n)?

No. Complexity depends on the operation and implementation.

## Q86. Is repeated concatenation always slow?

Do not make an absolute claim. Modern JS engines optimize many
concatenation patterns.

For large structured construction, this can be clear:

``` js
const parts = [];

for (const item of items) {
  parts.push(item);
}

const result = parts.join("");
```

## Q87. What should you consider with Unicode?

`.length`, indexing, spread, and grapheme segmentation measure different
concepts.

------------------------------------------------------------------------

# 36. Important String Interview Differences

  -----------------------------------------------------------------------
  Topic                               Key Difference
  ----------------------------------- -----------------------------------
  `at()` vs `charAt()`                `at()` supports negative indexes

  `slice()` vs `substring()`          Negative indexes and argument
                                      ordering differ

  `replace()` vs `replaceAll()`       First vs all literal occurrences

  `includes()` vs `indexOf()`         Boolean vs index

  `search()` vs `indexOf()`           Regex vs literal substring

  `match()` vs `matchAll()`           Match API vs iterator for all
                                      detailed matches

  `charCodeAt()` vs `codePointAt()`   UTF-16 unit vs Unicode code point

  `String()` vs `.toString()`         `String()` handles null/undefined

  `parseInt()` vs `Number()`          Prefix parsing vs whole-value
                                      conversion

  primitive vs `new String()`         primitive vs object

  `.length` vs grapheme count         UTF-16 units vs visible text
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 37. 3-Year Experience Rapid-Fire

1.  Are strings mutable? **No.**
2.  Does `trim()` mutate? **No.**
3.  Does `replace()` mutate? **No.**
4.  Does `toUpperCase()` mutate? **No.**
5.  What does `indexOf()` return when missing? **-1.**
6.  What does `includes()` return? **Boolean.**
7.  Is `includes()` case-sensitive? **Yes.**
8.  Does `replace()` replace all plain-string occurrences? **No.**
9.  What replaces all literal occurrences? **`replaceAll()` or regex
    with `g`.**
10. Does `split()` return an array? **Yes.**
11. Does `substring()` support negative indexes? **No.**
12. Does `slice()` support negative indexes? **Yes.**
13. Why is `"😀".length` 2? **UTF-16 code units.**
14. What does `codePointAt()` provide? **Unicode code point.**
15. What does `String.fromCodePoint()` do? **Creates a string from code
    points.**
16. What is `localeCompare()` used for? **Locale-aware
    comparison/sorting.**
17. What does `normalize()` help with? **Unicode normalization.**
18. What does `String.raw` do? **Provides raw template-literal text.**
19. What is `matchAll()` useful for? **All regex matches with capture
    groups.**
20. What is the `parseInt` + `map` trap? **The array index becomes the
    radix argument.**
21. Is `new String("")` falsy? **No, it is an object and truthy.**
22. Is `"false"` falsy? **No, non-empty strings are truthy.**
23. Does `Number("")` return NaN? **No, it returns 0.**
24. Does `Number("10px")` return 10? **No, NaN.**
25. Does `parseInt("10px", 10)` return 10? **Yes.**

------------------------------------------------------------------------

# 38. Final Interview Checklist

You should be comfortable with:

-   String immutability
-   Primitive vs String object
-   `length`
-   `at`
-   `charAt`
-   `charCodeAt`
-   `codePointAt`
-   `fromCharCode`
-   `fromCodePoint`
-   `concat`
-   `includes`
-   `startsWith`
-   `endsWith`
-   `indexOf`
-   `lastIndexOf`
-   `search`
-   `match`
-   `matchAll`
-   `replace`
-   `replaceAll`
-   `split`
-   `slice`
-   `substring`
-   `repeat`
-   `padStart`
-   `padEnd`
-   `trim`
-   `trimStart`
-   `trimEnd`
-   `toUpperCase`
-   `toLowerCase`
-   `normalize`
-   `localeCompare`
-   Unicode/UTF-16
-   Unicode code points
-   Grapheme clusters
-   `Intl.Segmenter`
-   Regex flags
-   Regex capture groups
-   Global regex `lastIndex`
-   Template literals
-   Tagged templates
-   String coercion
-   `String()` vs `.toString()`
-   `Number()` vs `parseInt()` vs `parseFloat()`
-   `parseInt()` + `map()` trap
-   String coding problems
-   Search/normalization
-   Slug generation
-   Input validation
-   XSS considerations
-   SQL injection considerations
-   String performance

------------------------------------------------------------------------

# 39. Final Rule

For every important string method, be ready to answer:

1.  What does it return?
2.  Does it create a new string?
3.  What happens on an empty string?
4.  What happens on an invalid index?
5.  Is it case-sensitive?
6.  Does it accept regex?
7.  What are its Unicode implications?
8.  What is its approximate complexity?
9.  What are its edge cases?
10. Is there a better specialized API for the production problem?

That level of explanation is much more valuable in a 3-year interview
than simply memorizing method syntax.
