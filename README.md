# Pythonic JavaScript
A document showing similarities (and differences) between Python and JavaScript code.

This document assumes you have a perfect browser that generally supports all of ES6. That SHOULD be every browser, as ECMAScript2015/ES6 is the current standard, but for now unfortunately not all browsers implement everything. I personally always use the somewhat glitchy browsers [Google Chrome Canary](https://www.google.com/chrome/browser/canary.html) and [Firefox Nightly](https://nightly.mozilla.org/)/[Developer Edition](https://www.mozilla.org/en-US/firefox/developer/). [Node.js 5](https://nodejs.org/en/blog/release/v5.0.0/) (generally with the `--harmony` flag) is also pretty good with ES6.

Keep in mind that effectively nobody has these browsers [i.e., very few people who will be using your site or program] and you should always use transpilers such as [Babel](https://babeljs.io/) or [Traceur](https://github.com/google/traceur-compiler).

You can use Babel right in your web browser (without the need of downloading or installing or compiling anything!) by using the [official online REPL](http://babeljs.io/repl/).

Python version 3.4 is used in the examples, except when otherwise mentioned.

**TODO:**
* Special functions:
  * Generators
  * [(View)](#python-decorators) ~~Python Decorators~~
  * [ES2016 JavaScript Class Property Decorators](https://github.com/wycats/javascript-decorators/), maybe?
* Array handling
  * View items
  * Push, pop, slice, splice, shift
* String handling
  * Slice, splice
  * Starts with, ends with
* Number handling
  * Rounding, floor/ceiling
* Uncategorized (as of yet):
  * JSON parsifying/stringifying

## Special functions

### Python Decorators

```python
def wrapper(f):
    def inner(*args, **kw):
        print "Hey, you called me!"
        return f(*args, **kw)
    return inner

@wrapper
def my_function(x):
    return x * 2

my_function(6) # "Hey, you called me!" gets printed and 12 is returned

# or, you can wrap my_function like this:
def my_function(x):
    return x * 2
my_function = wrapper(my_function)
```

```javascript
function wrapper(f) {
  function inner(...args) {
    console.log("Hey, you called me!");
    return f(...args);
  }
  return inner;
}

function myFunction(x) {
  return x * 2;
} myFunction = wrapper(myFunction);

myFunction(6); // "Hey, you called me!" gets logged and 12 is returned

// The above can be simplified with function expressions (and also
// arrow functions):

function wrapper(f) {
  return (...args) => {
    console.log("Hey, you called me!");
    return f(...args);
  };
}

const myFunction = wrapper(x => x * 2);
```

## Control Structures

Note that `for` loops in Python will behave as if they used a `var` statement in JavaScript, so after the loop has exited the final value of the name used in the loop will be accessable, like so:

```python
for item in ['a', 'b', 'c']:
    pass

print item # -> 'c'
```

```javascript
for (var item of ['a', 'b', 'c']);
console.log(item); // -> 'c'
```

However you would normally use `let` instead of `var` in a JavaScript loop statement as `let` helps gets rid of issues such as hoisting.

### Loop through items of a list/array:

```python
for item in [1, 2, 3]:
    print(item)
# -> 1
# -> 2
# -> 3
```

```javascript
for (let item of [1, 2, 3]) {
  console.log(item);
}
// -> 1
// -> 2
// -> 3
```

### Loop through keys of a dictionary/object:

```python
for key in {"x": 30, "y": 15}:
    print(key)
# -> y
# -> x

# properties order is reversed in python, hmm..
# normally this doesn't matter, but you can make
# them go from left to right like this:
# (yay for list slicing)
for key in list({'x': 30, 'y': 15})[::-1]:
    print(key)
# -> x
# -> y
```

[Read more](http://stackoverflow.com/q/509211/4633828) about Python list slicing!

```javascript
for (let key in {'x': 30, 'y': 15}) {
  console.log(key);
}
// -> x
// -> y

// or, replicate the reversed keys thing of Python
// like this:
for (let key of Object.keys({'x': 30, 'y': 15}).reverse()) {
  console.log(key);
}
// -> y
// -> x
// again, this shouldn't matter, but just in case.
```

### Loop through entries

The JavaScript `Object.entries` method is [part of ES7](https://github.com/tc39/proposal-object-values-entries).

```python
for index, item in enumerate(['a', 'b', 'c']):
    print(index, item)
# -> 0 a
# -> 1 b
# -> 2 c

for index, item in {'x': 42, 'y': False}.items():
    print(index, item)
# -> y False
# -> x 42

# in Python 2, use list#iteritems
```

```javascript
for (let [index, item] of ['a', 'b', 'c'].entries()) {
  console.log(index, item);
}
// -> 0 a
// -> 1 b
// -> 2 c

// objects don't have an "entries" method, only arrays -
// use the Object.entries method for objects and such:
for (let [key, value] of Object.entries({x: 42, y: false})) {
  console.log(key, value);
}
// -> x 42
// -> y false
```

## Array Handling

### Get indexes of a list/array:

```python
range(len(['a', 'b', 'c'])) # [0, 1, 2]
```

```javascript
['a', 'b', 'c'].map((_, i) => item) // [0, 1, 2]
```

### Reverse list/array:

```python
['a', 'b', 'c'][::-1] # ['c', 'b', 'a']
```

```javascript
['a', 'b', 'c'].reverse() // ['c', 'b', 'a']
```

## String Handling

### Split string

```python
'hello world'.split(' ') # ['hello', 'world']
```

```javascript
'hello world'.split(' ') // ['hello', 'world']
```

### Join strings

```python
'hello' + ' world' // 'hello world'
```

```javascript
'hello' + ' world' // 'hello world'
// or:
'hello'.concat(' world') // 'hello world'
```

## Number handling

### Add, subtract, multiply, divide, exponentiate

The JavaScript exponentiation operator (`**`) is part of ECMAScript 2016.

```python
3 - 2 # 1
3 + 2 # 5
3 * 2 # 6
3 / 2 # 1.5, or 1 if using Python 2

3 ** 2 # 9
# or:
pow(3, 2) # 9
```

```javascript
3 - 2 // 1
3 + 2 // 5
3 * 2 // 6
3 / 2 // 1.5

3 ** 2 // 9
// or:
Math.pow(3, 2)
```

### Modulo (remainder division)

```python
8 % 3 # 2
```

```javascript
8 % 3 // 2
```

## Convert Types

### Convert to list/array:

```python
list('hello') # ['h', 'e', 'l', 'l', 'o']
```

```javascript
Array.from('hello') // ['h', 'e', 'l', 'l', 'o']
```

### Convert to string:

```python
str(34) # '34'
```

```javascript
(34).toString() // '34'
```

### Convert to number:

Note that Python will raise a ValueError on converting a string literal containing a decimal such as "15.4", while JavaScript will return an integer rounded down (`'15.7'` -> `15`).

```python
int('15') # 15
float('34.75') # 34.75
```

```javascript
parseInt('15') // 15
parseFloat('34.75') // 34.75
```
